# Procedimiento: Story Split

Revisa y documenta una historia aprobada usando como fuente de referencia las
tareas que ya fueron creadas y enlazadas en Jira. No crees, dupliques ni
propongas nuevas tareas. Cada tarea existente DEBE seguir el template canónico
de `templates/task-template.md` al pie de la letra.


## Precondición

- La historia debe estar identificada por su key de Jira o por el texto pegado.
- Antes de analizar el corte, consulta en Jira las tareas ya relacionadas con la
  historia: enlaces de tipo `relates to`, subtareas, tareas hijas y cualquier
  otra relación explícita disponible.
- Si no se encuentran tareas relacionadas, detén el split y repórtalo como
  bloqueo. No crees tareas nuevas ni completes el contexto inventando.

## Proceso
1. Lee la historia (Jira por key en el campo descripción o history description,
   o texto pegado).
2. Obtén y registra las tareas relacionadas ya existentes, conservando su key,
   título, tipo, estado y relación con la HU. Esa lista es cerrada: el análisis
   debe trabajar únicamente sobre esas tareas.
3. Consulta codebase-memory MCP para mapear qué servicios/módulos del multi-repo
   se ven afectados. Usa ese análisis para validar y completar las tareas
   existentes; no lo uses para crear tareas adicionales.
4. Mapea cada requisito de la HU contra una o más tareas existentes e identifica
   cobertura, solapamientos, dependencias y vacíos. Un vacío debe reportarse
   como observación o bloqueo, nunca convertirse en una tarea nueva.
5. Mantén la separación existente entre frontend y backend. Las tareas de
   documentación deben quedar inmersas en las tareas existentes y no se deben
   crear tareas independientes para documentar.
6. Las reglas de negocio deben quedar en las tareas de backend; si hay
   validaciones compartidas entre frontend y backend, repórtalas en ambas
   tareas. En las reglas de negocio trata de conservar el texto de la HU
   principal.
7. Completa el template con información REAL. No dejes secciones vacías: si no hay
   información, escribe "N/A — confirmar con producto/TL".
8. En Consideraciones técnicas usa nombres reales (endpoints, tablas, flags) desde
   codebase-memory cuando se conozcan.
9. En Dependencias lista explícitamente las tareas hermanas existentes que van
   antes, usando sus keys de Jira.


## Output obligatorio

Primero muestra un resumen de trazabilidad entre la HU y las tareas existentes,
indicando para cada requisito qué tarea lo cubre y señalando cualquier vacío o
solapamiento.

Después, entrega una sección por cada tarea existente. No agregues secciones para
tareas hipotéticas o faltantes. Usa EXACTAMENTE la estructura de
`templates/task-template.md`:
Contexto · Objetivo · Alcance · Reglas de negocio · Criterios de aceptación ·
Consideraciones técnicas (APIs, BD, Performance, Seguridad, Feature Flags) ·
Dependencias · Riesgos · Referencias (Figma, Confluence, Swagger, ADR) ·
Definition of Done.

## Escritura en Jira
Muestra el output completo para revisión. No crees tareas, subtareas ni issues
nuevos bajo ninguna circunstancia durante este procedimiento. No dupliques una
tarea existente. Cualquier vacío, cambio de alcance o necesidad de una nueva
tarea debe quedar reportado para revisión del TL/producto.
