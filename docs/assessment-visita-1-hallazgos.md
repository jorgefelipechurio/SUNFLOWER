# Assessment inicial — Visita N.º 1

**Proyecto:** Revamping y automatización — Planta de aceite de girasol (~30 años de antigüedad)
**Elaborado por:** Ing. Jorge Churio
**Fecha:** Septiembre de 2026

## Situación actual
Planta a lazo abierto: ajuste de parámetros por observación/medición manual, actuando sobre variadores. Acciones correctivas y evaluación de estado por intervención humana exclusivamente.

## Contexto (entrevista con gerente de planta)
En marcha un plan de incorporación de equipos (complementa, no reemplaza el equipamiento existente), que incluye un sistema SCADA.

## Hallazgos por etapa

| Etapa | Situación actual | Riesgo | Mejora propuesta | Prioridad |
|---|---|---|---|---|
| 1. Silos de entrada | Mezcla de capas (humedad/aceite) por lote, manual | Mezcla subóptima, sin registro sistemático | Registro electrónico por silo/lote + software de optimización de mezcla sobre sinfines | Media |
| 2. Molino | Velocidad ajustada manualmente sobre variador según color (% reingreso) y nivel de tolva | Ajuste subjetivo, variabilidad de proceso | Sensores de color + nivel, lazo PID continuo con alarmas de borde | **Alta (crítica)** |
| 3. Extrusor | Temp. de salida y flujo determinan calidad (riesgo de quemado); sin realimentación con molino | Degradación de producto | Sensor de temperatura → control de velocidad del extrusor, realimentado al PID del molino (lazos anidados) | **Alta (crítica)** |
| 4. Prensas | Control manual de luz de prensa vs. presión máxima | Fatiga/rotura del tornillo | Actuador en registro de apertura para mantener carga en rango aceptable | Media |
| 5. Depósito de expeller | Sin detección de sobrecalentamiento | Fermentación/combustión | Sensores térmicos y/o detectores de humo | Alta (seguridad) |

## Conclusiones
- SCADA requiere datos de proceso; el equipamiento actual no tiene sensado. Se recomienda instrumentar los puntos críticos relevados, no depender solo de sensores de los equipos nuevos (se asume que sí los tendrán).
- No todas las mejoras son igual de críticas: **molino y extrusor (lazos cerrados) son la máxima prioridad.**

## Limitaciones
Alcance del plan de upgrade/reemplazo de líneas y maquinaria aún no definido al momento de este assessment.

## Próximos pasos sugeridos
1. Definir con Gerencia el alcance final del plan de equipos + SCADA.
2. Priorizar lazos PID de molino y extrusor (fase 1).
3. Especificar/cotizar instrumentación de campo (color, nivel, temperatura, presión/carga, humo).
4. Relevar en próxima visita tableros eléctricos, variadores existentes y disponibilidad de señales.

---
Informe ejecutivo completo (Word, firmado): *Informe Ejecutivo - Assessment Visita 1 - Planta Girasol.docx* (entregado al usuario el 15/09/2026).
