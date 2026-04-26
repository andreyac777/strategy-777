---
tags: [estrategia, presets, backtesting]
updated: 2026-04-26
---

# Presets — Configuraciones Validadas

> [!info] Uso recomendado
> Usar estos valores como punto de partida al iniciar una nueva sesión de pruebas. No optimizar desde cero — partir del preset más cercano al activo y TF que vas a operar.

---

## PRESET-001 — Configuración Base

> [!success] Validado en backtesting
> **Fecha:** 2026-04-17 · **Activo:** Pendiente confirmar (par forex) · **TF:** Por confirmar

| Parámetro | Valor |
|-----------|-------|
| Min Breakout (pips) | 2 |
| Min Break Candle (pips) | 20 |
| Min Prev Candle (pips) | 20 |
| Extra SL (pips) | 10 |
| RR Ratio | 1.8 |
| Max TP Distance (pips) | 160 |
| Pip Size | 1 |
| Bars Until New Entry | 5 |
| Bars After SL/TP | 4 |
| Min Between Red Boxes (pips) | 40 |
| Min Between Green Boxes (pips) | 40 |
| Session Start (hour) | 6 |
| Session End (hour) | 12 |
| UTC Offset | -6 |
| Debug Zones | false |

> [!note] Notas
> - Sesión 6–12 = horario Colombia (UTC-6), equivale a 12:00–18:00 UTC
> - Min Break Candle y Min Prev Candle en 20 pips es más permisivo que el default de 60
> - Extra SL en 10 pips da más holgura para evitar salidas por ruido

---

## Agregar Nuevo Preset

> [!tip] Plantilla
> Cuando encuentres una configuración que funcione, registrarla con:
> - Fecha y activo/TF
> - Tabla de valores
> - Condiciones del mercado en las que se validó (tendencia, rango, alta/baja volatilidad)
