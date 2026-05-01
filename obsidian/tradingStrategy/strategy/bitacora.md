# Bitacora de Evolucion de la Estrategia TLS

Historial de cambios y metricas del backtest. Cada fila = un commit con sus resultados.

**Convencion:**
- Profit USD/% positivo = mejora vs capital inicial
- DD % = drawdown maximo (menor es mejor)
- PF = profit factor (>1 = profitable, ideal >1.5)
- Win % = porcentaje de operaciones rentables
- Δ vs anterior = diferencia en profit USD respecto a la entrada previa

| Fecha      | Commit        | Cambio                                                                                                                                | Trades | Profit USD | Profit % | Win %  | DD %  | PF   | Δ vs anterior       |
| ---------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------------- | ------ | ---------- | -------- | ------ | ----- | ---- | ------------------- |
| 2026-05-01 | d9278ed       | Multi-tier pivots (macro/medium/micro) + slot system rework: cascada eliminada, BK solo en 1A/1B, segmentos cortados por intersección | 188    | +6,028.16  | +0.60%   | 38.83% | 0.38% | 1.20 | — (primera entrada) |
| 2026-05-01 | [FEATURE WIP] | Filtro duro HTF room (1.5 ATR). Mejora #2 tier proximity probada y revertida (catastrófica: -4,896 USD, win 32.74%)                    | 195    | +6,428.41  | +0.64%   | 38.97% | 0.38% | 1.27 | +400.25 USD ↑       |
