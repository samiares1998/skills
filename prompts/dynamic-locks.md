# Procedimiento: Migrar configuración estática a Dinamic Locks

Analiza un listado, filtro o regla estática existente y propón o implementa su
migración al módulo de bloqueos dinámicos por perfil.

## Objetivo

Reemplazar configuraciones hardcodeadas como:

```ts
const STATUS_BY_ROLE = {
  ANALYST: 'CREATED,PROCESSING_HOLD_FAILED',
  ADMIN: 'CREATED,APPROVED,PROCESSING_HOLD_FAILED',
};
```

por una configuración administrable mediante categorías, opciones de acción y
asignaciones a perfiles.

## Contrato de Dinamic Locks

Usa únicamente el contrato real entregado por el usuario o verificado en el
código. Como referencia, el consumo de opciones asignadas por perfil y
categoría es:

```text
GET {orchestrator-base-url}/lock-options/dinamic/profile/{profileCode}/category/{categoryCode}/option
```

Parámetros:

- `profileCode`: código del perfil o rol del usuario autenticado.
- `categoryCode`: código de la categoría funcional, por ejemplo `LBTR` o
  `VALE_VISTA`.

La respuesta contiene asignaciones `ProfileActionOptionHttpDTO[]`. Cada
asignación contiene un `actionOption`; para filtros construidos por estados,
usa `actionOption.code` como valor del estado.

Ejemplo conceptual:

```json
[
  {
    "profile_code": "SSO-BO-BANK-ADMIN",
    "action_option": {
      "code": "CREATED"
    }
  },
  {
    "profile_code": "SSO-BO-BANK-ADMIN",
    "action_option": {
      "code": "PROCESSING_HOLD_FAILED"
    }
  }
]
```

El filtro resultante debe construirse como:

```text
CREATED,PROCESSING_HOLD_FAILED
```

No uses `metadata` para representar estados cuando el usuario indique que los
estados se modelan mediante `actionOption.code`. No inventes categorías,
opciones, endpoints ni campos adicionales.

## Entradas esperadas

El usuario puede entregar una o varias de estas entradas:

- Archivo, componente, hook o función que contiene el listado estático.
- Código del listado o filtro que debe hacerse dinámico.
- Repositorio y módulo afectados.
- Código de categoría dinámica.
- Endpoint que consume el módulo funcional, por ejemplo historial LBTR o Vale
  Vista.
- Perfiles involucrados y su comportamiento esperado.
- Indicación de si desea solo análisis, propuesta o implementación.

Si falta el `categoryCode`, el significado de `actionOption.code` o la regla de
precedencia entre perfiles, déjalo como pregunta pendiente. Puedes avanzar con
el análisis, pero no inventes el valor.

## Proceso

1. Inspecciona el archivo y el flujo real donde se usa la configuración
   estática.
2. Identifica:
   - listado, mapa o condición hardcodeada;
   - perfil o rol usado actualmente;
   - endpoint funcional que recibe el filtro;
   - forma exacta del parámetro, por ejemplo `status` separado por comas;
   - manejo actual de loading, errores y ausencia de configuración.
3. Verifica si ya existe un cliente de Dinamic Locks. Reutilízalo si cumple el
   contrato; si no existe, crea un cliente dedicado siguiendo las convenciones
   del repositorio.
4. Implementa o propone una función de dominio separada por módulo, por
   ejemplo:

   ```ts
   getLbtrStatusByProfile(profileCode)
   getValeVistaStatusByProfile(profileCode)
   ```

   Cada función debe fijar su propia categoría y consultar el cliente común.
5. Convierte las opciones asignadas en el filtro requerido por el endpoint
   funcional:
   - toma `actionOption.code`;
   - elimina valores vacíos;
   - elimina duplicados sin alterar el orden recibido;
   - une los valores con el separador que exige el contrato funcional;
   - retorna configuración ausente cuando no hay opciones válidas.
6. Integra la consulta dinámica en el hook o servicio responsable del módulo.
   La vista no debe conocer URLs ni transformar DTOs.
7. Mantén separados los adaptadores de LBTR, Vale Vista u otros módulos. Solo
   comparte el cliente HTTP y utilidades genéricas.
8. Maneja explícitamente:
   - perfil sin código;
   - respuesta vacía;
   - error `400` de validación;
   - error `404` de categoría, perfil u opción;
   - error `401/403` de autenticación o autorización;
   - errores `5xx` o de red.
9. Agrega o actualiza pruebas para el cliente, el resolver y el hook. Incluye
   al menos un caso con dos códigos asignados a un mismo perfil.
10. Ejecuta las validaciones disponibles y reporta cualquier fallo previo o no
    relacionado con el cambio.

## Reglas de implementación

- No elimines el flujo legacy sin confirmar que el usuario desea reemplazarlo.
- Si deben convivir legacy y dinámico, documenta cuál tiene prioridad y dónde
  se decide.
- No mantengas una tabla de estados por rol como fuente de verdad después de
  migrar ese módulo a Dinamic Locks.
- Usa `encodeURIComponent` para `profileCode` y `categoryCode` en la URL.
- No guardes en frontend una lista fija de estados que debe venir del backend.
- Si se requiere fallback temporal al comportamiento estático, hazlo explícito,
  configurable y reporta que sigue existiendo una fuente de verdad duplicada.
- No modifiques servicios, endpoints o repositorios externos sin autorización
  explícita.
- Preserva cambios no relacionados del usuario.

## Salida obligatoria

Entrega exactamente estas secciones:

### 1. Diagnóstico del código actual

- Archivo(s) y función(es) involucradas.
- Configuración estática encontrada.
- Endpoint funcional que recibe el filtro.
- Evidencia observada.
- Supuestos y preguntas pendientes.

### 2. Diseño propuesto

- Categoría usada por el módulo.
- Forma de representar cada filtro como `actionOption.code`.
- Flujo `perfil → categoría → opciones asignadas → filtro funcional`.
- Tratamiento de ausencia de configuración y errores.
- Convivencia o prioridad entre legacy y dinámico, si aplica.

### 3. Cambios realizados o propuesta de cambios

Lista archivos, responsabilidades y modificaciones. Si no se pidió
implementación, entrega pseudocódigo o diff sugerido, sin editar archivos.

### 4. Configuración esperada en Dinamic Locks

Incluye una tabla:

| Categoría | Código de opción | Perfil(es) | Resultado esperado |
|---|---|---|---|
| | | | |

### 5. Pruebas y validaciones

- Pruebas agregadas o ejecutadas.
- Resultado de TypeScript/lint/build.
- Casos no cubiertos.

### 6. Preguntas pendientes

Incluye solo las preguntas necesarias para cerrar decisiones de producto o
arquitectura.

## Seguridad

Este procedimiento puede leer e implementar cambios en el repositorio indicado,
pero no debe crear, modificar o publicar información en Jira, GitLab,
Confluence u otro sistema externo sin confirmación explícita.
