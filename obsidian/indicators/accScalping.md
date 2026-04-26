---
tags:
  - indicador
  - strategy
  - pine-script
  - scalping
archivo: acc-scalping.pine
updated: 2026-04-26
cssclasses:
---

# ACC Scalping

Estrategia principal del proyecto. Genera señales de compra/venta basadas en pares de velas (techo rojo / piso verde) con zonas dinámicas que se extienden hacia la derecha.

→ Ver reglas completas en [[../strategy/goalAndPhilosophy]] y [[../strategy/entryRules]]

---

## Motor de la Estrategia

### Techo Rojo (señal de venta)
- Una vela **roja** sobrepasa y cierra **por debajo** del extremo inferior del cuerpo de la vela verde anterior
- Caja: techo = mecha superior del par | piso = extremo inferior del cuerpo anterior
- No se construye si una vela del par toca una caja anterior sin haberla roto

### Piso Verde (señal de compra)
- Una vela **verde** sobrepasa y cierra **por arriba** del extremo superior del cuerpo de la vela roja anterior
- Caja: techo = extremo superior del cuerpo anterior | piso = mecha inferior del par
- No se construye si una vela del par toca una caja anterior sin haberla roto

### Duración de las cajas
- Se extienden dinámicamente hacia la derecha por cada nueva vela
- **Techo rojo** deja de extenderse si una vela verde cierra con su cuerpo sobre la caja
- **Piso verde** deja de extenderse si una vela roja cierra con su cuerpo por debajo de la caja

---

## Parámetros Principales

| Parámetro | Descripción |
|---|---|
| `i_minBreak` | Pips mínimos de rompimiento |
| `i_minCandlePips` | Tamaño mínimo de la vela que rompe |
| `i_minPrevPips` | Tamaño mínimo de la vela anterior |
| `i_cooldown` | Barras mínimas entre entradas |
| `i_postTrade` | Barras de espera post SL/TP |
| `i_sesStart / i_sesEnd` | Ventana de sesión operativa |
| `i_utcOffset` | Offset UTC para sesión local |

→ Preset validado en [[../strategy/presets#PRESET-001]]

---

## Gestión de Riesgo

- **Stop Loss:** pips de la vela que rompió + margen configurable (`Extra SL pips`)
- **Take Profit:** definido por ratio R/R configurable
- **Máximo TP:** distancia máxima configurable en pips (`i_maxTPDist`)

---

## Estado

| Ítem | Estado |
|---|---|
| Zonas dinámicas | ✅ Implementado |
| Cooldown / postTrade | ✅ Implementado |
| Filtro de sesión | ✅ Implementado |
| Tabla RR Analysis | ✅ Implementado |
| HTF Trend Filter | ⏳ Pendiente — ver [[../strategy/pandora#BUG-001]] |

---

## Bugs Conocidos

- **BUG-001:** Fake signals en contratendencia → [[../strategy/pandora#BUG-001]]
- **BUG-002:** Tabla RR recomienda ratio que no coincide con backtesting real → [[../strategy/pandora#BUG-002]]
