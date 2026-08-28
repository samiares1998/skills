# Procedimiento: Deuda técnica

Prepara una propuesta de backlog para registrar una deuda técnica a partir del
contexto entregado por el TL. El resultado debe ayudar a decidir si la deuda se
registra como una HU de deuda técnica independiente o si se divide como una o
más subtareas de una HU existente. No crees ni modifiques elementos en Jira.

## Fuentes obligatorias

Aplica siempre las reglas de `prompts/tenpo-backlog-rules.md`.

Si el TL entrega una clave o URL de Jira, consulta la HU relacionada, su épica,
subtareas y criterios de aceptación. Si entrega un repositorio, servicio,
módulo, endpoint, clase, tabla o archivo, consulta el código real mediante
codebase-memory MCP para ubicar la deuda y sus impactos. Usa grep o búsqueda de
archivos solo para literales, configuración o archivos que no estén indexados.

No inventes contexto faltante. Registra como pregunta pendiente cualquier dato
que no pueda verificarse.

## Entradas esperadas

El TL puede entregar una o varias de estas entradas:

- Contexto y síntoma de la deuda.
- Ubicación: repositorio, servicio, módulo, endpoint, clase, tabla, flujo o
  archivo.
- HU, épica o clave/URL de Jira relacionada, si existe.
- Motivación: riesgo, incidente, mantenibilidad, seguridad, performance,
  obsolescencia o costo operativo.
- Restricciones, dependencias, prioridad y urgencia conocidas.

Si no se entrega una HU relacionada, busca una relación verificable en Jira o
en el código. Si no existe, indícalo explícitamente.

## Proceso

1. Resume la deuda en lenguaje claro y separa hechos observados, supuestos y
   preguntas pendientes.
2. Identifica la ubicación técnica exacta y los componentes afectados.
3. Explica el impacto actual y el riesgo de no resolverla.
4. Determina si está vinculada a una HU existente, a una épica o a ninguna.
5. Evalúa las dos alternativas:
   - **HU independiente de deuda técnica**: cuando tiene objetivo, alcance,
     criterios de aceptación y valor técnico propios, o cuando no depende de una
     entrega funcional concreta.
   - **Subtarea(s) de una HU existente**: cuando es una actividad necesaria y
     acotada para completar esa HU, comparte su alcance y no constituye un
     entregable independiente.
6. Recomienda una alternativa, justificándola con trazabilidad, tamaño,
   independencia, riesgo y capacidad de verificación. La recomendación no
   reemplaza la decisión del TL.
7. Prepara el borrador correspondiente sin crear nada externamente.

## Criterios de decisión

- La deuda técnica debe clasificarse como **RUN**, salvo que el contexto
  demuestre otra clasificación según las reglas oficiales.
- Una HU independiente debe seguir el formato:
  `Yo como <usuario técnico o equipo>` / `Quiero <acción o resultado>` /
  `Para <beneficio técnico o de negocio>`.
- Una subtarea no debe contener un entregable gigante ni existir sin HU padre.
- La propuesta debe ser estimable, verificable y suficientemente pequeña para
  el nivel elegido.
- No conviertas una decisión arquitectónica no aprobada en un hecho. Si falta
  la decisión del TL, deja la alternativa abierta y enlázala como pregunta.

## Salida obligatoria

Entrega exactamente estas secciones:

### 1. Resumen de la deuda
- Descripción
- Ubicación técnica
- Evidencia observada
- Impacto actual
- Riesgo de no resolverla

### 2. Trazabilidad
- HU relacionada: clave, título y vínculo, o `No identificada`
- Épica relacionada: clave, título y vínculo, o `No identificada`
- Relación comprobada
- Dependencias

### 3. Evaluación de alternativas

| Alternativa | Encaje | Ventajas | Riesgos o limitaciones |
|---|---|---|---|
| HU independiente de deuda técnica | | | |
| Subtarea(s) de HU existente | | | |

### 4. Recomendación para decisión del TL

Indica una de estas opciones: `Crear HU independiente`, `Dividir como
subtarea(s)` o `Información insuficiente`. Explica brevemente por qué y qué
dato podría cambiar la recomendación.

### 5. Borrador de HU de deuda técnica

Inclúyelo siempre que sea viable:

- **Título**
- **Clasificación RGT**: RUN
- **Descripción**: Yo como / Quiero / Para
- **Alcance**: incluye y no incluye
- **Criterios de aceptación**: en formato Gherkin
- **Estimación sugerida**: Fibonacci o `N/A — confirmar con TL`
- **Dependencias**
- **Riesgos**
- **Referencias técnicas**
- **Definition of Done**

### 6. División sugerida

Si la alternativa de subtareas es viable, lista las subtareas propuestas con
nombre, objetivo, alcance, dependencia y criterio de término. No las crees en
Jira.

### 7. Preguntas pendientes

Lista únicamente preguntas necesarias para cerrar la propuesta; no completes
las respuestas por inferencia.

## Seguridad

Entrega primero el resultado completo para revisión. Solo después de una
confirmación explícita del TL se podrá crear o modificar la HU o sus subtareas
en Jira.
