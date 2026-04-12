# Requisitos de Backtesting

## Objetivo
Evaluar la estrategia en histórico para medir:
- rentabilidad,
- win rate,
- consistencia,
- drawdown,
- robustez.

## Temporalidades
Debe funcionar en:
- 1m, 5m, 15m
- 1h, 4h
- diario

## Multi-Timeframe
Debe permitir:
- pruebas en distintas temporalidades,
- opcional uso de confirmación HTF,
- comparación de resultados.

## Métricas Requeridas
- win rate
- total de trades
- profit neto
- profit bruto
- pérdidas
- profit factor
- promedio por trade
- drawdown
- long vs short

## Historial
Debe permitir ver:
- trades cerrados,
- entradas/salidas,
- timestamps,
- resultados por trade.

## Ventana de Backtesting
Debe ser configurable.

### Recomendación:
- intradía: 3–6 meses
- swing: 1–3 años

Debe incluir:
- mercados en rango,
- tendencias,
- alta/baja volatilidad.

## Consistencia
No evaluar solo profit:
- estabilidad,
- clustering de pérdidas,
- drawdown,
- consistencia.

## Futuro
Posible expansión:
- paneles personalizados,
- métricas mensuales,
- separación long/short.
