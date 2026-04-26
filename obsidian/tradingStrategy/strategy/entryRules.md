---
tags: [estrategia, entradas, filtros]
updated: 2026-04-26
---

# Reglas de Entrada

## Cuándo Entrar

> [!success] Condición de entrada válida
> Con una **vela de continuación**, indistintamente si está dentro o fuera de la caja.
> - Después de **caja roja** → entrar en **ventas (bearish)**
> - Después de **caja verde** → entrar en **compras (bullish)**

---

## Cuándo NO Entrar

> [!warning] Filtros de invalidación
> | Condición | Parámetro |
> |-----------|-----------|
> | Muy pocos pips de rompimiento | `i_minBreak` |
> | Vela anterior al rompimiento muy pequeña | `i_minPrevPips` |
> | Vela de rompimiento muy pequeña | `i_minCandlePips` |
> | No se cumplió el cooldown desde la última entrada | `i_cooldown` |
> | No pasaron suficientes barras después de un SL o TP | `i_postTrade` |
> | Precio fuera de la sesión operativa | `i_sesStart`, `i_sesEnd`, `i_utcOffset` |

---

## Parámetros en el Código

| Parámetro | Descripción |
|-----------|-------------|
| `i_minBreak` | Pips mínimos de rompimiento para validar el setup |
| `i_minCandlePips` | Tamaño mínimo en pips de la vela que rompe |
| `i_minPrevPips` | Tamaño mínimo en pips de la vela anterior al rompimiento |
| `i_cooldown` | Barras mínimas entre entradas |
| `i_postTrade` | Barras de espera después de cerrar un trade (SL o TP) |
| `i_sesStart` | Hora de inicio de sesión (hora local) |
| `i_sesEnd` | Hora de cierre de sesión (hora local) |
| `i_utcOffset` | Diferencia con UTC para calcular hora local |

---

## Filtros Pendientes

> [!bug] BUG-001 — HTF Trend Filter pendiente
> Setup válido técnicamente pero el mercado va en dirección contraria.
> **Propuesta:** filtro de tendencia HTF activable con `request.security()`.
> → Detalle completo en [[pandora#BUG-001]]

---

## Preset Validado

> [!tip] Punto de partida
> → Ver [[presets#PRESET-001]] para valores que funcionan en backtesting.
