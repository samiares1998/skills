# Procedimiento: Branch Review (2 agentes en paralelo)

Review de los cambios de una rama local en dos dimensiones **independientes**,
ejecutadas **en paralelo** por dos agentes que no se ven entre sí:

- **Agente A — Calidad de código**: ¿el código está bien hecho?
- **Agente B — Cumplimiento de la tarea**: ¿el cambio resuelve *exactamente* lo que pide la tarea de Jira?

Se separan a propósito: un código impecable que resuelve otra cosa igual se rechaza,
y un código que cumple el criterio de aceptación pero rompe un contrato también.
Si un solo agente evalúa ambas cosas, tiende a justificar una con la otra.

---

## Parámetros (todos opcionales)

El flujo se puede invocar sin nada. Formas aceptadas:

| Invocación | Rama a revisar (`FEATURE`) | Base (`BASE`) |
|---|---|---|
| `branch-review` | rama actual (HEAD) | autodetectada |
| `branch-review <base>` | rama actual (HEAD) | `<base>` |
| `branch-review <base>..<feature>` o `<base>...<feature>` | `<feature>` | `<base>` |
| `branch-review <feature> contra <base>` (o `vs`, `into`, `->`) | `<feature>` | `<base>` |
| `branch-review --base=<b> --branch=<f>` | `<f>` | `<b>` |
| `branch-review JIRA=<ID>` (combinable con lo anterior) | — | — |

Resolución de argumentos:
- **Un solo argumento** ⇒ es la **base**. Es el caso frecuente ("revisá contra release/2.4")
  y evita el ambiguo "¿esta rama es la base o la feature?". Si el TL quiere pasar solo la
  feature, debe usar la forma explícita `--branch=`.
- Si un argumento no corresponde a una rama existente (`git rev-parse --verify`), no
  adivinar: probar también `origin/<arg>` y, si tampoco existe, **preguntar** listando
  las ramas parecidas.
- `JIRA=<ID>` fuerza la tarea a revisar e ignora lo que diga el nombre de la rama
  (útil cuando la rama no sigue la convención).

### Defaults cuando no vienen parámetros

- `FEATURE` = rama actual: `git rev-parse --abbrev-ref HEAD`.
  Si HEAD está en la base misma (o detached), avisar y detener: no hay nada que revisar.
- `BASE` = primera que exista, en este orden:
  1. `main`
  2. `master`
  3. la rama por defecto del remoto: `git symbolic-ref refs/remotes/origin/HEAD`
  Si ninguna existe, preguntar al TL. **No** asumir `develop` en silencio: si el repo
  usa git-flow, el TL la pasa como parámetro.
- Si existe `origin/<BASE>` y está más actualizada que la local, usar `origin/<BASE>`
  y decirlo en el informe (evita comparar contra una base vieja sin fetch).

Siempre **declarar en el encabezado del informe** qué rama y qué base se usaron y si
fueron explícitas o por default. El TL tiene que poder detectar una comparación errónea
de un vistazo.

---

## Fase 0 — Contexto (lo hace el orquestador, antes de lanzar los agentes)

1. **Resolver parámetros** (`FEATURE`, `BASE`, `JIRA` si vino) según la sección anterior.

2. **Diff**
   - Punto de divergencia: `git merge-base <BASE> <FEATURE>`
   - Diff: `git diff --stat <merge-base>...<FEATURE>` y `git diff <merge-base>...<FEATURE>`
   - Commits: `git log --oneline <merge-base>..<FEATURE>`
   - Si el diff viene vacío, detenerse y reportarlo (base equivocada o rama ya mergeada).

3. **ID de la tarea desde el nombre de la rama** (si no vino `JIRA=`)
   Patrón: `<tipo>/<PROJ>-<num>-<slug>` → ej. `feat/CHLO-123-fixes` ⇒ `CHLO-123`.
   Regex sugerida: `[A-Z][A-Z0-9]+-\d+`. Se extrae de `FEATURE`, no de HEAD.
   - Si hay varios matches, usar el primero y avisarlo.
   - Si **no hay match**, no inventar: preguntar el ID al TL antes de continuar.
     Sin ID, el Agente B no puede correr (el A sí).

4. **Tarea de Jira**
   Traer vía Jira MCP: título, descripción, criterios de aceptación (Gherkin si los hay),
   subtareas, comentarios relevantes y el issue padre (HU/Épica) si existe, porque el
   alcance real suele estar ahí.

5. **Lanzar A y B en paralelo**, pasándoles a ambos: rama, base, merge-base, lista de
   archivos tocados y el diff. Solo a B se le pasa además la tarea de Jira.
   Ambos usan **codebase-memory MCP** para entender el código que rodea al cambio
   (convenciones del repo, contratos entre servicios, dónde más se usa lo que se tocó).

---

## Agente A — Calidad de código

Revisa **solo el código**. No sabe qué pide la tarea y no debe intentar adivinarlo:
si algo parece fuera de alcance, lo reporta como observación, no como veredicto.

Checklist (mismo criterio que `prompts/mr-review.md`):
Corrección (bugs, null/Optional en Java) · Tests (caminos de error, edge cases,
¿hay tests nuevos para el código nuevo?) · Concurrencia/estado (JVM: races,
mutabilidad compartida) · Manejo de errores y logging · Contratos/API (breaking para
otros servicios — cruzar con codebase-memory) · Seguridad (secretos, validación de
input, autorización) · Performance (N+1, loops, recursos sin cerrar) · Convenciones
del repo · Duplicación de algo que ya existe en otro servicio.

Salida:
- Hallazgos por severidad 🔴 / 🟡 / 🔵, cada uno con `archivo:línea` y el porqué.
- ⚖️ decisiones de diseño/arquitectura que **no decide la IA**.
- Qué no pudo revisar (código no indexado, side effects fuera del diff, etc.).

## Agente B — Cumplimiento de la tarea

Revisa el diff **contra la tarea de Jira**. No opina de estilo ni de calidad.

1. Extrae de la tarea la lista explícita de criterios de aceptación. Si no están
   formalizados, deriva los requisitos del texto y **marca que fueron inferidos**.
2. Para cada criterio, busca en el diff (y con codebase-memory en el código que lo
   rodea) la evidencia concreta que lo implementa: archivo, función, test.
3. Clasifica cada criterio: ✅ cubierto · 🟠 parcial · ❌ no cubierto · ❓ no verificable
   desde el código (ej. requiere config, feature flag o cambio en otro repo).
4. Detecta **alcance extra**: cambios en el diff que no responden a ningún criterio.
   No son necesariamente malos (refactor oportunista), pero deben quedar explícitos.
5. Detecta **contradicciones**: el código hace algo distinto de lo que pide el criterio.

Salida:
- Tabla criterio → estado → evidencia (`archivo:línea`) → comentario.
- Lista de alcance extra.
- Lista de ambigüedades de la tarea que impiden verificar (son pregunta para producto,
  no invento de la IA).

---

## Fase final — Consolidación (orquestador)

Junta ambos reportes y emite **un solo informe**:

```
# Branch Review — <FEATURE> (<JIRA-ID>: <título de la tarea>)
Comparación: <FEATURE> ← <BASE>   [explícita | default]
Merge-base: <sha corto> · Commits: <n> · Archivos: <n> (+X/-Y)

## Veredicto: <✅ Aprobar | 🟡 Aprobar con comentarios | 🔴 Cambios requeridos>
<2-3 líneas: por qué>

## 1. Cumplimiento de la tarea
| Criterio | Estado | Evidencia | Comentario |
Cobertura: N/M criterios cubiertos.
### Alcance extra
### Ambigüedades para producto

## 2. Calidad del código
### 🔴 Bloqueantes
### 🟡 Sugerencias
### 🔵 Nits

## 3. ⚖️ Para juicio del TL
Decisiones de diseño/arquitectura + cruces entre ambos reportes
(ej. "cumple el criterio pero rompe el contrato con servicio-X").

## 4. Límites de esta revisión
Qué no se pudo verificar y por qué.
```

Reglas del veredicto:
- Cualquier 🔴 del Agente A ⇒ **Cambios requeridos**.
- Cualquier ❌ en un criterio de aceptación ⇒ **Cambios requeridos**.
- Solo 🟠 / 🟡 / 🔵 ⇒ **Aprobar con comentarios**.
- El veredicto es una recomendación: la decisión final es del TL.

## Reglas de seguridad
- **NO** escribir en Jira (comentarios, transiciones) ni publicar en GitLab sin
  confirmación explícita del TL.
- **NO** modificar código: este flujo es de solo lectura.
- Si falta contexto (criterios vagos, ID ausente, repo sin indexar), se declara;
  no se completa inventando.
