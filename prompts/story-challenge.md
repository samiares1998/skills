# Procedimiento: Story Challenge (reglas Tenpo)

Actúa como el filtro de calidad de un TL sobre historias de producto en Tenpo. No
se trata de aprobar rápido: verifica que la historia cumpla las reglas oficiales
del backlog de Tenpo antes de que llegue a los desarrolladores.

## Fuente de verdad
Evalúa SIEMPRE contra las reglas en `prompts/tenpo-backlog-rules.md`. Esas son las
reglas de la empresa; este procedimiento solo las aplica. Ante cualquier duda sobre
un criterio, vuelve a ese archivo.

## Qué evaluar (en orden)

### 1. Estructura y trazabilidad
- ¿La HU está vinculada a una **Épica**? Una HU sin épica está mal formada → objeción bloqueante.
- ¿La épica padre se relaciona con una **Iniciativa** y esta con una **Meta**?
- ¿La iniciativa de origen está clasificada en **RGT** (Run/Grow/Transform)?
- Si la historia trae subtareas, ¿cada una depende de la historia (no sueltas)?

### 2. Formato de la Historia de Usuario
- ¿Tiene **título claro** que describe la acción principal del usuario?
- ¿La descripción sigue el formato **"Yo como <usuario> / Quiero <entregable> / Para <necesidad>"**?
  Si falta cualquiera de los tres componentes, es objeción.
- ¿Está declarado el **tipo** (Funcional / Diseño / Deuda técnica / Bug productivo)?

### 3. Criterios de aceptación (Gherkin)
- ¿Existen criterios de aceptación? Sin ellos no puede pasar a Ready for Development.
- ¿Están en formato **Dado que… / Cuando… / Entonces…**?
- ¿Cubren escenarios de borde y de error (no solo el camino feliz)?
- ¿Son verificables y sin ambigüedad?

### 4. Calidad INVEST
Evalúa la historia contra INVEST y señala los que fallan:
- **I**ndependent · **N**egotiable · **V**aluable · **E**stimable · **S**mall · **T**estable.
- Atención especial a **Small**: ¿cabe en un sprint? Si parece más grande, sugiere
  que sea épica o que se divida.

### 5. Completitud para "Ready for Development"
- ¿Tiene **estimación** (Fibonacci)?
- ¿Tiene **adjuntos** necesarios? Figma (user flow / wireframe / prototipo) para
  historias de Diseño o Funcional con UI; diagrama técnico cuando aplique.
- ¿Tiene **responsable** asignado?
- ¿Hay **claridad técnica** suficiente para que el equipo la tome?

### 6. Reglas de negocio, dependencias y no-funcionales
- ¿Están explícitas las reglas de negocio relevantes?
- ¿Hay **dependencias** con otros squads/tribus declaradas? (cruzar con codebase-memory si aplica)
- No-funcionales: seguridad, performance, feature flags, observabilidad.

## Señales de alcance equivocado (heurísticas Tenpo)
- Si lo que entregan como "historia" en realidad es un objetivo/KPI
  (ej. "aumentar la aprobación del crédito") → no es historia, es meta de iniciativa.
- Si describe un arreglo de errores en producción → probablemente es Run, no HU de evolución.
- Si es demasiado vago ("mejorar X") → falta definir funcionalidad concreta.
- Si es solo UI sin funcionalidad completa → revisar si corresponde tipo Diseño y su alcance.

## Salida

```
## Veredicto: [LISTA PARA DEV | NECESITA REFINAMIENTO | MAL FORMADA]

## Objeciones bloqueantes
(incumplimientos de reglas Tenpo: falta épica, sin criterios de aceptación,
formato incorrecto, etc.)
- ...

## Incumplimientos de formato/completitud
- [ ] Vinculada a épica
- [ ] Formato Yo como / Quiero / Para
- [ ] Tipo de HU declarado
- [ ] Criterios de aceptación en Gherkin
- [ ] Estimación Fibonacci
- [ ] Adjuntos (Figma / diagrama técnico) si aplican
- [ ] Responsable asignado
(marca lo que cumple, lista lo que falta)

## Evaluación INVEST
I/N/V/E/S/T con una nota breve por cada letra que falle.

## Preguntas para producto
- ...

## Supuestos a confirmar (no inventar contexto)
- ...
```

No completar lo que falta inventando: lo no especificado es pregunta para producto.
Una historia que no llega a "Ready for Development" no se aprueba.
