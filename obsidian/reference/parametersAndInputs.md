---
tags: [referencia, parametros, inputs]
updated: 2026-04-26
---

# Parámetros e Inputs

Todo lo importante debe ser **configurable desde inputs**. Agrupar por categoría.

---

## Grupos de Inputs

| Grupo | Parámetros |
|-------|------------|
| **Trend** | MA length, tipo de MA, confirmación HTF |
| **Entry** | Distancia de rompimiento, sensibilidad de retest, tamaño de vela, tolerancia de mechas |
| **Exit** | SL, TP, trailing stop |
| **Risk** | Ratio R/R, lógica dinámica |
| **Session** | Sesiones, horarios, días |
| **Filters** | Volatilidad (ATR), thresholds |
| **Visual** | Labels, zonas, transparencia |
| **Debug** | Activar/desactivar debug |

---

## Principios

- Cada parámetro relevante debe ser configurable
- Los filtros deben poder activarse/desactivarse individualmente
- Debe ser fácil agregar nuevos inputs sin romper la base

---

## Parámetros de ACC Scalping

| Parámetro | Tipo | Descripción |
|-----------|------|-------------|
| `i_minBreak` | int | Pips mínimos de rompimiento |
| `i_minCandlePips` | int | Tamaño mínimo de la vela que rompe |
| `i_minPrevPips` | int | Tamaño mínimo de la vela anterior |
| `i_cooldown` | int | Barras mínimas entre entradas |
| `i_postTrade` | int | Barras de espera post SL/TP |
| `i_sesStart` | int | Hora inicio sesión (local) |
| `i_sesEnd` | int | Hora fin sesión (local) |
| `i_utcOffset` | int | Offset UTC |
| `i_maxTPDist` | int | Distancia máxima de TP en pips |
| `i_useHTF` | bool | Activar filtro HTF (pendiente) |
