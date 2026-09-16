# SUNFLOWER — Revamping y automatización, Planta de Aceite de Girasol

Repositorio de referencia (fuente de verdad) del proyecto **Valeriano Sunflower**: automatización y modernización de una planta de aceite de girasol de ~35 años de antigüedad, operada a lazo abierto. Se evalúan mejoras en el ciclo de prensado/extracción, instrumentación y lazos de control en etapas críticas, y se analiza estado del arte para modernización de plantas *brownfield*.

**Responsable:** Ing. Jorge Churio (jorge.f.churio@gmail.com)
**Última actualización:** Septiembre 2026

---

## 1. Estado actual de la planta

Planta a lazo abierto: ajuste de parámetros por observación/medición manual sobre variadores, sin sensado sistemático. Hay un plan de incorporación de equipos en marcha (complementa el equipamiento existente) que incluye SCADA — pero el SCADA necesita datos de proceso, y hoy no hay sensado en los puntos críticos.

### Hallazgos por etapa (Visita 1)

| Etapa | Situación actual | Riesgo | Mejora propuesta | Prioridad |
|---|---|---|---|---|
| 1. Silos de entrada | Mezcla de capas (humedad/aceite) por lote, manual | Mezcla subóptima, sin registro sistemático | Registro electrónico por silo/lote + software de optimización de mezcla sobre sinfines | Media |
| 2. Molino | Velocidad ajustada manualmente sobre variador según color (% reingreso) y nivel de tolva | Ajuste subjetivo, variabilidad de proceso | Sensores de color + nivel, lazo PID continuo con alarmas de borde | **Alta (crítica)** |
| 3. Extrusor | Temp. de salida y flujo determinan calidad (riesgo de quemado); sin realimentación con molino | Degradación de producto | Sensor de temperatura → control de velocidad del extrusor, realimentado al PID del molino (lazos anidados) | **Alta (crítica)** |
| 4. Prensas | Control manual de luz de prensa vs. presión máxima | Fatiga/rotura del tornillo | Actuador en registro de apertura para mantener carga en rango aceptable | Media |
| 5. Depósito de expeller | Sin detección de sobrecalentamiento | Fermentación/combustión | Sensores térmicos y/o detectores de humo | Alta (seguridad) |

**Conclusión clave:** molino y extrusor (lazos cerrados) son la máxima prioridad. Detalle completo → [`docs/assessment-visita-1-hallazgos.md`](docs/assessment-visita-1-hallazgos.md).

---

## 2. Línea de trabajo activa: precalentamiento de semilla antes del extrusor

A partir del hallazgo del extrusor como etapa crítica, se investigó si precalentar la semilla antes de esa etapa mejora rendimiento y reduce el riesgo de sobrecalentamiento del producto.

**Conclusión:** sí, está bien documentado en la literatura (no es solo intuición de planta) y existen fabricantes dedicados a este tipo de equipo. Además se evaluó una idea de innovación propia — un **precalentador tipo sinfín con calentamiento por microondas** — con precedentes académicos de diseño (aunque a escala de laboratorio/piloto, no producto de catálogo). También se agregó un dimensionamiento preliminar de cavidad y potencia para el caso de 100 Tn/día.

Documento completo (mecanismos, evidencia académica, fabricantes, consideraciones de diseño del sinfín por microondas, dimensionamiento preliminar, caso de negocio) → [`docs/precalentamiento-semillas-preheater-referencias.md`](docs/precalentamiento-semillas-preheater-referencias.md).

### Resultado central del caso de negocio (planta de 100 Tn/día)

Usando el estudio específico de girasol (Gürdil et al. 2020: eficiencia de expresión de aceite de 87% → 93.46% al precalentar a 80°C):

| Escenario | Mejora | Fuente | Aceite adicional/día | Adicional/año (300 días) | Valor bruto adicional/año |
|---|---|---|---|---|---|
| Conservador | +4.3% | Gaber et al. 2020 (canola) | +1.65 Tn | ≈494 Tn | ≈USD 723.000 |
| **Medio (girasol)** | **+7.4%** | **Gürdil et al. 2020** | **+2.84 Tn** | **≈853 Tn** | **≈USD 1.249.000** |
| Optimista | +10% | Koubaa et al. 2016 (colza) | +3.83 Tn | ≈1.148 Tn | ≈USD 1.682.000 |

Supuestos y limitaciones completos (a reemplazar por datos reales de planta) en el documento y en la planilla de cálculo. El valor bruto **no** incluye CAPEX/OPEX del precalentador — aún sin cotizar.

### Dimensionamiento preliminar del sinfín con microondas (caso 100 Tn/día, 4 min de residencia)

| Parámetro | Valor de referencia |
|---|---|
| Volumen de cavidad | ≈2,4 m³ (densidad 380 kg/m³, llenado 30%) |
| Potencia térmica mínima | ≈73 kW (Cp≈0,97 kJ/kg·K, ΔT=65K) |
| Potencia con FS 1,5× | ≈109 kW |
| Potencia instalada estimada (con eficiencia de acoplamiento 50-70%) | ≈156-218 kW |

Detalle y fuentes en la sección 7.1 de [`docs/precalentamiento-semillas-preheater-referencias.md`](docs/precalentamiento-semillas-preheater-referencias.md).

---

## 3. Entregables (`/deliverables`)

| Archivo | Descripción |
|---|---|
| [`Pitch_Precalentador_Microondas_Girasol.pdf`](deliverables/Pitch_Precalentador_Microondas_Girasol.pdf) / [`.pptx`](deliverables/Pitch_Precalentador_Microondas_Girasol.pptx) | Pitch deck (11 slides): hallazgo, mejora propuesta, estado del arte y fabricantes actuales, ventajas comparativas del microondas, sustento académico, evidencia cuantitativa y hoja de ruta. |
| [`Caso_Negocio_Precalentamiento_Girasol.xlsx`](deliverables/Caso_Negocio_Precalentamiento_Girasol.xlsx) | Planilla de cálculo con fórmulas vivas: supuestos editables (volumen, % aceite, eficiencia, precio, días/año), los 3 escenarios, y referencias académicas al pie. |
| [`OnePager_Caso_Negocio_Precalentamiento_Girasol.pdf`](deliverables/OnePager_Caso_Negocio_Precalentamiento_Girasol.pdf) | Resumen ejecutivo de una página: supuestos, situación actual, los 3 escenarios y referencias. |

---

## 4. Documentos fuente (`/docs`)

| Archivo | Contenido |
|---|---|
| [`assessment-visita-1-hallazgos.md`](docs/assessment-visita-1-hallazgos.md) | Hallazgos completos de la Visita 1: situación por etapa, riesgos, mejoras propuestas, prioridades, próximos pasos. |
| [`precalentamiento-semillas-preheater-referencias.md`](docs/precalentamiento-semillas-preheater-referencias.md) | Documento técnico completo: mecanismos, evidencia académica, cómo funcionan los precalentadores convencionales, microondas y sinfín propio, dimensionamiento preliminar, fabricantes, caso de negocio a 100 Tn/día, próximos pasos. |

---

## 5. Próximos pasos (consolidado)

1. Definir con Gerencia el alcance final del plan de equipos + SCADA.
2. Priorizar lazos PID de molino y extrusor (fase 1).
3. Especificar/cotizar instrumentación de campo (color, nivel, temperatura, presión/carga, humo).
4. Relevar en próxima visita tableros eléctricos, variadores existentes y disponibilidad de señales.
5. Precalentamiento — camino convencional: cotizar 2-3 fabricantes (ver sección 8 del documento técnico).
6. Precalentamiento — camino microondas: contactar integradores industriales (Industrial Microwave Systems, SAIREM, Muegge) para evaluar factibilidad de un sinfín a medida; validar primero en banco de pruebas con semilla propia. Usar como punto de partida el dimensionamiento preliminar (sección 7.1).
7. Medir en planta la eficiencia de extracción real y el % de aceite de la semilla procesada, para reemplazar los supuestos del caso de negocio por datos propios.
8. Encargar ensayo dieléctrico de la semilla propia antes de cerrar el dimensionamiento del sinfín con microondas.

---

## 6. Cómo usar este repositorio

Este repositorio es la **fuente de verdad** del proyecto: cualquier hallazgo, decisión o entregable durable debería quedar reflejado acá (en `/docs` si es un análisis/hallazgo, en `/deliverables` si es un archivo para compartir). Al iterar sobre un documento existente, actualizar el archivo en `/docs` completo (no crear versiones paralelas) y reflejar el cambio relevante en este README si afecta el resumen ejecutivo.

## 7. Nota sobre los archivos binarios de /deliverables

Los 4 archivos de la carpeta `deliverables/` (2 PDF, 1 PPTX, 1 XLSX) se suben en un paso aparte por una restricción técnica de la sesión de automatización de navegador. Si aún no aparecen en `deliverables/`, están disponibles en el chat de Claude para descarga y subida manual.
