# Procedimiento: MR Review

Primer pase de review de un MR de GitLab. Exhaustivo en lo mecánico, honesto en lo
que no se puede juzgar.

## Proceso
1. Lee el MR vía GitLab MCP (diff, descripción, pipeline).
2. Cruza con codebase-memory: convenciones, contratos entre servicios, duplicación.

## Checklist
Corrección (bugs, null/Optional en Java) · Tests (caminos de error, edge cases) ·
Concurrencia/estado (JVM: races, mutabilidad compartida) · Manejo de errores ·
Contratos/API (breaking para otros servicios) · Seguridad · Performance (N+1,
loops, recursos sin cerrar) · Convenciones del repo.

## Output por severidad
🔴 Bloqueante · 🟡 Sugerencia · 🔵 Nit · ⚖️ Para juicio del TL (decisiones de
diseño/arquitectura que no decide la IA).

NO publicar comentarios en GitLab sin confirmación explícita.
