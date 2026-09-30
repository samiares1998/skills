# Invocar flujos usando lenguaje natural en Codex

Este documento contiene ejemplos para usar los procedimientos de `skills`
desde Codex sin memorizar comandos específicos.Se debe configurar un MCP
para jira y para todos los repos del bo

## Refinar o desafiar una historia

```text
Evalúa la siguiente historia usando prompts/story-challenge.md y
prompts/tenpo-backlog-rules.md. Indica si está lista para Ready for Development.

[pegar aquí la historia o indicar la clave/URL de Jira]
```

## Obtener contexto de una HU usando tareas ya creadas

```text
Obtén el contexto completo de esta HU usando el skill story-context.

Consulta en Jira las tareas que ya están enlazadas o relacionadas explícitamente
con la HU y úsalas como fuente de referencia. Basándote en la HU y en el título
de cada tarea, redacta la descripción de cada una siguiendo exactamente
templates/task-template.md; incorpora allí el contexto funcional y técnico, la
trazabilidad, las dependencias, los riesgos y los vacíos.

Consulta el código real para identificar servicios afectados, dependencias,
APIs, tablas y riesgos cuando sea necesario. No dividas la HU, no propongas ni
crees tareas nuevas y no modifiques Jira.

[pegar aquí la historia o indicar la clave/URL de Jira]
```

## Generar descripciones técnicas de tareas existentes

```text
Usa el skill task-description para redactar la descripción de estas tareas Jira
a partir de la HU indicada. Consulta Codebase Memory para verificar servicios,
endpoints, clases, tablas, eventos y dependencias reales cuando corresponda.
Entrega un bloque por tarea usando exactamente templates/task-template.md.
No crees tareas nuevas, no dividas la HU y no modifiques Jira.

HU:
[clave o URL de la HU]

Tareas existentes:
[claves, URLs o títulos de las tareas]

Nombre de los repositorios involucrados:
[nombres]

```

## Analizar y preparar una deuda técnica

```text
Analiza la siguiente deuda técnica usando prompts/technical-debt.md y
prompts/tenpo-backlog-rules.md.

Te entregaré el contexto y la ubicación técnica. Identifica la HU o épica
relacionada, si existe, y recomienda si conviene crear una HU independiente de
deuda técnica o dividirla como subtarea(s) de una HU existente. Prepara ambos
borradores cuando sea posible para que yo tome la decisión.

Consulta el código real cuando indique repositorio, servicio, módulo, endpoint,
clase, tabla o archivo. No inventes información faltante y no crees ni
modifiques nada en Jira sin mi confirmación explícita.

Contexto:
[describir la deuda técnica]

Ubicación:
[indicar repositorio, servicio, módulo, endpoint, clase, tabla o archivo]

HU/épica relacionada, si existe:
[pegar clave o URL]
```

## Revisar una rama local

```text
Revisa los cambios de mi rama actual contra main siguiendo
prompts/branch-review.md.

Haz el análisis de calidad del código y verifica el cumplimiento de la tarea de
Jira asociada. Entrega un informe consolidado con severidades y veredicto.
No modifiques código ni publiques comentarios.
```

También puedes especificar las ramas:

```text
Revisa la rama feat/CHLO-123-fixes contra release/2.4 siguiendo
prompts/branch-review.md. Solo lectura.
```

## Revisar un Merge Request

```text
Revisa el Merge Request indicado usando prompts/mr-review.md.

Analiza los cambios por severidad, entrega feedback accionable y no publiques
comentarios en GitLab sin mi confirmación explícita.

[pegar aquí la URL o el ID del MR]
```

## Documentar un flujo end-to-end

```text
Documenta el flujo end-to-end descrito a continuación usando
prompts/flow-doc.md.

Usa el código indexado para identificar servicios, endpoints, eventos,
contratos, persistencia y errores. Incluye un diagrama Mermaid cuando aplique.

[describir aquí el flujo o indicar el endpoint/evento inicial]
```

## Migrar una configuración estática a Dinamic Locks

```text
Migra la siguiente configuración estática a Dinamic Locks usando
prompts/dynamic-locks.md.

Consulta el código real del repositorio y separa hechos observados, supuestos y
preguntas pendientes. Usa el endpoint por perfil y categoría:

GET /lock-options/dinamic/profile/{profileCode}/category/{categoryCode}/option

Los estados deben salir de actionOption.code, no de metadata. Mantén separados
los adaptadores de cada módulo, por ejemplo LBTR y Vale Vista. Si implementas el
cambio, agrega pruebas y reporta las validaciones. No modifiques Jira, GitLab o
Confluence.

Repositorio:
[nombre o ruta del repositorio]

Módulo o archivo:
[ruta, componente, hook o función]

Configuración estática actual:
[pegar código o describirla]

Categoría dinámica:
[por ejemplo LBTR o VALE_VISTA]

Perfiles involucrados:
[códigos de perfil]
```

## Crear o actualizar un ADR

```text
Prepara un ADR usando prompts/adr.md para esta decisión arquitectónica.

Investiga primero el estado real del código y separa hechos observados,
supuestos, alternativas y decisión propuesta. No inventes contexto faltante.

[describir aquí la decisión]
```


## Regla de seguridad

Para Jira, GitLab o Confluence, Codex debe preparar primero el resultado y
esperar confirmación explícita antes de crear, modificar, comentar o publicar
información externamente.

Si falta información de producto, negocio o arquitectura, debe indicarlo como
pregunta pendiente en lugar de completarla inventando.

```
