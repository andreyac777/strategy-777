# Bitacora de Evolucion de la Estrategia TLS

Historial de cambios y metricas del backtest. Cada fila = un commit con sus resultados.

**Convencion:**
- Profit USD/% positivo = mejora vs capital inicial
- DD % = drawdown maximo (menor es mejor)
- PF = profit factor (>1 = profitable, ideal >1.5)
- Win % = porcentaje de operaciones rentables
- Δ vs anterior = diferencia en profit USD respecto a la entrada previa

| Fecha      | Commit  | Cambio                                                                                                                                | Trades | Profit USD | Profit % | Win %  | DD %  | PF   | Δ vs anterior       |
| ---------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | -------- | ------ | ----- | ---- | ------------------- |
| 2026-05-01 | d9278ed | Multi-tier pivots (macro/medium/micro) + slot system rework: cascada eliminada, BK solo en 1A/1B, segmentos cortados por intersección | 188    | +6,028.16  | +0.60%   | 38.83% | 0.38% | 1.20 | — (primera entrada) |
