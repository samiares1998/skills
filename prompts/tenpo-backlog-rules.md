# Reglas de Backlog de Tenpo

Reglas oficiales de construcción y gestión del backlog (Taller de Backlog Tenpo).
Esta es la fuente de verdad que usan los procedimientos de challenge y split de
historias. Está organizada por nivel de la jerarquía.

## Principios generales del backlog
- Toda iniciativa del Backlog Transversal debe clasificarse como **Run / Grow / Transform (RGT)**.
- Claridad sobre propósito y valor de cada ítem.
- Priorización basada en impacto, urgencia y esfuerzo.
- Ítems suficientemente detallados según cercanía a ejecución.
- Visibilidad para todos los roles.
- Sin trabajo "por fuera" del backlog (nada de trabajo oculto).


## Jerarquía y trazabilidad (regla estructural clave)
Iniciativa → Épica → Historia de Usuario → Subtarea.
- Cada nivel debe estar enlazado al de arriba.
- Una HU sin Épica asociada está mal formada.
- Una Subtarea sin historia padre está mal formada.
- Todo debe quedar enlazado de forma transversal y visible en Jira (tablero One Tenpo / por squad).

## Modelo RGT (clasificación de iniciativas)
- **RUN**: mantener continuidad del negocio y salud del sistema. Incidentes,
  correcciones, deuda técnica que habilita estabilidad. Tipos: integraciones,
  cambios normativos, deuda técnica. (La deuda técnica vive en RUN pero se mide
  explícitamente en el backlog transversal.)
- **GROW**: mejorar y escalar capacidades existentes; valor incremental. Tipos:
  nuevas funcionalidades, mejoras en journey, eficiencia en performance.
- **TRANSFORM**: iniciativas estratégicas que cambian el modelo o habilitan nuevas
  capacidades. Tipos: innovación estratégica, nuevas capacidades core, nuevos modelos.

## Nivel INICIATIVA
Pieza estratégica de alto nivel que apalanca una meta en un esfuerzo concreto,
agrupando varias épicas. Debe considerar:
- Descripción breve de la oportunidad.
- Meta u objetivo al que responde (Tribu/Squad).
- Resultados esperados / KPIs asociados.
- Alcance general (qué incluye / qué no incluye).
- Horizonte: 1, 3 o hasta 6 meses.
- Tribus/Squads involucrados.
Reglas: enfocadas y "delgadas"; siempre ligada a una meta formal; un único
responsable (ownership); descomponible en épicas pequeñas; dependencias
declaradas desde el inicio; no convertirla en "proyecto eterno".
Bien: "Reducir el tiempo de aprobación del crédito en 30% en 3 meses".
Mal: "Mejorar la app" / "Hacer cambios en la home" (vagas, sin meta ni KPI).

## Nivel ÉPICA
Traduce una iniciativa en un entregable grande, claro y accionable end-to-end.
Debe considerar:
- Descripción clara del objetivo (resultado esperado al finalizar).
- Conexión explícita con la iniciativa y la estrategia (outcome al que contribuye).
- Alcance definido: incluye / no incluye / dependencias.
- Criterios de éxito = Definition of Done a nivel épica.
- Segmentación inicial: HU clave, prioridad, riesgos.
- Estimación de esfuerzo (tallas de camiseta: S/M/L/XL).
- Trazabilidad en Jira (espacio de squad, enlazada a iniciativa y meta).
Reglas:
- Debe terminar en un **resultado observable (outcome)**, no solo un output técnico.
- No mezclar evolución de producto + run en una misma épica.
- Evitar épicas gigantes: si tarda más de **6–8 semanas, dividir**.
- Cada épica mueve **una sola métrica (KPI) principal**.
- Un único dueño (PDL/Squad).
Bien: "Gestión de métodos de pago", "Simulador de crédito", "Sistema de notificaciones push".
Mal: "Aumentar la aprobación del crédito" (es objetivo, no épica); "Arreglar errores
del sistema de crédito" (es Run); "Mejorar el simulador" (vago); "Prevenir fraude"
(demasiado amplio, son varias épicas); "Rediseñar la pantalla de pagos" (es UI, no
funcionalidad completa).

## Nivel HISTORIA DE USUARIO
Define QUÉ se construye, QUIÉN lo solicita y PARA QUÉ. Escrita por el PDL antes
del sprint. Asociada siempre a su épica.

Formato de descripción (obligatorio):
  Yo como <usuario>
  Quiero <entregable/acción>
  Para <necesidad/beneficio>

Tipos de HU: Funcional · Diseño · Deuda técnica · Bug productivo.

Elementos requeridos para crear la HU:
- Título claro: describe la acción principal del usuario.
- Descripción estandarizada en el formato Yo como / Quiero / Para.
- Criterios de aceptación (ver formato Gherkin abajo).
- Vinculación a su épica correspondiente.
- Estimación: Fibonacci.
- Adjuntos: Figma (user flow, wireframe, prototipo) y documentación técnica
  (diagrama técnico) cuando aplique.
- Responsable: equipo o persona asignada.

Criterios de aceptación — formato Gherkin (obligatorio):
  Escenario [número] [título del escenario]
  Dado que [contexto] y adicionalmente [contexto],
  Cuando [evento],
  Entonces [resultado / comportamiento esperado].
Cada HU está asociada a uno o más criterios de aceptación.

Calidad de la HU — checklist INVEST:
- **I**ndependent: independiente de otras historias.
- **N**egotiable: negociable, no un contrato cerrado.
- **V**aluable: aporta valor al usuario/negocio.
- **E**stimable: se puede estimar.
- **S**mall: pequeña; cabe en un sprint.
- **T**estable: tiene criterios verificables.

## Nivel SUBTAREA
Divide una HU en acciones concretas y ejecutables (tamaño ~diario).
- Cada subtarea es una actividad concreta, no un entregable gigante.
- Permiten seguimiento real: inicio → trabajo → resolución.
- Evitan "trabajo oculto" fuera del tablero.
- Pequeñas y accionables; evitar genéricas como "hacer desarrollo".
- Alineadas al workflow del squad.
- Responsable claro por subtarea.
- No existen sin una historia padre.

## Workflow del producto en Jira (estados)
Ideación → Ready for Development → Desarrollo → (Bloqueado) → Finalizada → Producción.
- **Ready for Development** significa: cumple criterios de aceptación, tiene
  claridad técnica y está lista para que el equipo la tome. Este es el umbral que
  una historia debe alcanzar para considerarse "lista".
