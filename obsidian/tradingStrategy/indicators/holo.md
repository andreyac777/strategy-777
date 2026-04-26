---
tags: [indicador, microestructura, pine-script]
archivo: holo.pine
updated: 2026-04-26
---

# Holo — Microestructura de Mercado

> [!abstract] Concepto
> Indicador overlay (Pine Script v6) que combina **cinco módulos independientes** para emitir una señal de COMPRAR / VENDER / ESPERAR con puntuación de 1 a 5.

---

## Los Cinco Módulos

| # | HUD | Variable | Qué mide |
|---|-----|----------|----------|
| 1 | Flujo | `ofiNormalizado` | Desequilibrio acumulado compras vs ventas (OFI normalizado) |
| 2 | D.Intelig. | `idxDineroInt` | Acumulación / distribución institucional (ROC precio + volumen) |
| 3 | Estructura | `microTendencia` | Secuencia de fractales — máximos y mínimos ascendentes o descendentes |
| 4 | Liquidez | `esVacioLiq` | Vacío de liquidez — rango amplio con bajo volumen |
| 5 | Divergencia | `hasDivergencia` | Desacuerdo entre regresión lineal de precio y volumen |

---

## Reglas de Señal

### COMPRAR

> [!success] Long — mínimo obligatorio: Flujo + Estructura + ≥ 1 más (3/5)
> | Condición | Variable | Obligatoria |
> |-----------|----------|-------------|
> | cL1 — Flujo comprando | `ofiNormalizado > 1` | ✅ SÍ |
> | cL2 — Institucional acumulando | `idxDineroInt == 1` | no |
> | cL3 — Estructura alcista | `microTendencia > 2` | ✅ SÍ |
> | cL4 — Liquidez libre arriba | `not esVacioLiq` o vacío sobre precio | no |
> | cL5 — Sin divergencia | `not hasDivergencia` | no |

### VENDER

> [!danger] Short — mínimo obligatorio: Flujo + Estructura + ≥ 1 más (3/5)
> | Condición | Variable | Obligatoria |
> |-----------|----------|-------------|
> | cS1 — Flujo vendiendo | `ofiNormalizado < -1` | ✅ SÍ |
> | cS2 — Institucional distribuyendo | `idxDineroInt == -1` | no |
> | cS3 — Estructura bajista | `microTendencia < -2` | ✅ SÍ |
> | cS4 — Vacío de liquidez abajo | `esVacioLiq[1] and mid[1] < close` | no |
> | cS5 — Sin divergencia | `not hasDivergencia` | no |

### ESPERAR / NO OPERAR

> [!warning] Cancelar señal si cualquiera de estas condiciones se cumple
> | Situación | Por qué no operar |
> |-----------|-------------------|
> | `idxDineroInt == 0` | Sin respaldo institucional |
> | `hasDivergencia` | Volumen contradice al precio |
> | `microTendencia` en rango (-2 a 2) | Sin dirección definida |
> | Conflicto `cL1 and cS2` ó `cS1 and cL2` | Módulos en sentidos opuestos |

---

## Puntuación en el HUD

> [!info] Lectura del panel
> ```
> ◉ COMPRAR   |   4/5 condiciones   → alta probabilidad
> ◉ COMPRAR   |   3/5 condiciones   → mínimo aceptable
> ◯ ESPERAR   |   —
> ⚡ CONFLICTO |   —
> ```
> - **3/5** → entrada válida
> - **4–5/5** → setup de alta probabilidad

---

## Parámetros Clave

| Parámetro | Default | Efecto |
|-----------|---------|--------|
| `smartMoneyLength` | 20 | Sensibilidad de módulos 2 y 5 |
| `accumThreshold` | 70% | Umbral acumulación/distribución |
| `liquidityThreshold` | 1.5× ATR | Tamaño mínimo del vacío |
| `fractalPeriod` | 5 | Velocidad de reacción de estructura |
| `structureDepth` | 3 | Fractales usados para `microTendencia` |

---

## Temas Visuales

`Aeon` · `Cyber` · `Quantum` · `Neural` · `Plasma` · `Crystal`
