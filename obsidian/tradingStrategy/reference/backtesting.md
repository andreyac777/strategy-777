---
tags: [referencia, backtesting, metricas]
updated: 2026-04-26
---

# Backtesting — Requisitos y Métricas

---

## Temporalidades Soportadas

`1m` · `5m` · `15m` · `1h` · `4h` · `D`

---

## Métricas Requeridas

| Métrica | Descripción |
|---------|-------------|
| Win rate | % de trades ganadores |
| Total de trades | Cantidad total de operaciones |
| Profit neto | Ganancia total después de comisiones |
| Profit bruto | Ganancia sin descontar pérdidas |
| Profit factor | Ganancias brutas / pérdidas brutas |
| Promedio por trade | Ganancia/pérdida media por operación |
| Drawdown | Caída máxima desde el pico de equity |
| Long vs Short | Separación de métricas por dirección |

---

## Ventana Recomendada

| Tipo | Ventana mínima | Debe incluir |
|------|----------------|--------------|
| Intradía (1m–15m) | 3–6 meses | Tendencias y rangos |
| Swing (1h–D) | 1–3 años | Alta/baja volatilidad |

---

## Criterios de Consistencia

No evaluar solo profit total. Revisar:

- Estabilidad de la curva de equity
- Clustering de pérdidas (¿están concentradas?)
- Drawdown máximo y duración
- Consistencia de win rate en diferentes períodos

---

## Configuración Base del Script

```pine
strategy(title="...",
     initial_capital=10000,
     default_qty_type=strategy.percent_of_equity,
     default_qty_value=1,
     commission_type=strategy.commission.percent,
     commission_value=0.05,
     slippage=2,
     max_bars_back=500,
     pyramiding=0)
```

---

## Expansión Futura

- Paneles personalizados de métricas
- Métricas mensuales / semanales
- Separación long/short en tabla
