---
tags: [estrategia, filosofia, motor]
updated: 2026-04-26
---

# Objetivo y Filosofía

## Filosofía Central

- No copiar estrategias — **ingeniería inversa** para entender qué genera ventaja
- No solo automatizar — **investigar**: ¿cuándo entrar? ¿cuándo NO? ¿qué filtros ayudan?
- Arquitectura **extensible desde el día uno**
- Primero analítico, luego operativo

---

## Motor de la Estrategia (ACC Scalping)

### 1. Techo Rojo → señal de venta

Una vela **roja** sobrepasa y cierra **por debajo** del extremo inferior del cuerpo de la vela verde anterior.

**Construcción de la caja:**
- Techo = mecha superior del par (vela verde + vela roja)
- Piso = extremo inferior del cuerpo de la vela verde (anterior al par)

**Restricción:** no se construye si alguna vela del par toca una caja anterior sin haberla roto.

### 2. Piso Verde → señal de compra

Una vela **verde** sobrepasa y cierra **por arriba** del extremo superior del cuerpo de la vela roja anterior.

**Construcción de la caja:**
- Techo = extremo superior del cuerpo de la vela roja (anterior al par)
- Piso = mecha más inferior del par (vela roja + vela verde)

**Restricción:** no se construye si alguna vela del par toca una caja anterior sin haberla roto.

### 3. Duración de las cajas (dinámicas)

| Tipo | Se extiende | Deja de extenderse cuando |
|------|-------------|--------------------------|
| Techo rojo | Hacia la derecha en cada vela | Una vela verde cierra con su cuerpo **sobre** la caja |
| Piso verde | Hacia la derecha en cada vela | Una vela roja cierra con su cuerpo **debajo** de la caja |

### 4. Cuándo entrar y cuándo NO

→ Ver [[entryRules]]

### 5. Gestión de salidas

- **Stop Loss:** pips de la vela que rompió + margen extra configurable
- **Take Profit:** ratio R/R configurable (1:1 o 1:2 como base)

---

## Objetivos del Framework

| Capacidad | Detalle |
|---|---|
| Señales | Long / Short / ambos (configurable) |
| Entradas | Tendencia, rompimientos, retests, momentum, volatilidad, estructura |
| Salidas | SL, TP, trailing stop, señal contraria, temporal/estructural |
| Riesgo | SL/TP configurables, ratio R/R, lógica dinámica |
| Filtros | Sesiones, horarios, días, ventanas sin trading |
| Backtesting | 1m a diario, multi-timeframe, métricas completas |
| Visual | Cajas de riesgo/beneficio, etiquetas de resultado — ver [[visualStyle]] |

---

## Reglas de Desarrollo

1. Claude **no inventa reglas de trading** — el usuario define la lógica exacta
2. Código modular, legible y preparado para crecer
3. Cada parámetro relevante debe ser configurable desde los inputs
4. Los filtros deben poder activarse/desactivarse individualmente
5. La base nunca se rompe al agregar funcionalidades nuevas
