---
tags: [indicador, trendlines, pine-script]
archivo: trendlines.pine
updated: 2026-04-26
---

# Trendlines

Indicador overlay (Pine Script v6) que dibuja trendlines automáticos basados en el par de pivotes que genera la línea con **más toques** entre los pivotes recientes.

---

## Concepto

De todos los pivotes altos recientes, busca el par de puntos que forma la línea con **mayor número de coincidencias** (toques de otros pivotes). Lo mismo para pisos.

Resultado: **1 resistencia + 1 soporte**, extendidas a la derecha, que se actualizan automáticamente al llegar un nuevo pivote.

---

## Algoritmo de Mejor Ajuste

1. Guarda los últimos `nPivots` techos y pisos por separado
2. Prueba **todos los pares posibles** como candidatos (O(N²))
3. Para cada par, cuenta cuántos otros pivotes están dentro de `tolerancia × ATR`
4. Dibuja la línea del par con más toques
5. En empate: gana el par más reciente
6. Se recalcula solo al detectar un nuevo pivote

---

## Parámetros

| Parámetro | Default | Descripción |
|-----------|---------|-------------|
| `pivPeriod` | 10 | Barras a cada lado para confirmar pivote |
| `nPivots` | 10 | Pivotes analizados (4–12) |
| `atrMult` | 0.5 | Tolerancia de toque en múltiplos de ATR |
| `colH` | `#00bfff` | Color resistencia |
| `colL` | `#ff5252` | Color soporte |
| `lineW` | 2 | Grosor |

---

## Calibración por Temporalidad

| TF | `pivPeriod` | Notas |
|----|-------------|-------|
| 1 min | 5 | Pivotes frecuentes |
| 5 min | 7 | |
| **15 min** | **10** | Default recomendado |
| 1 h | 10 | Reducir `nPivots` a 6–8 |
| 4 h | 8 | Pivotes escasos, subir tolerancia |

---

## Alertas Incluidas

- Nuevo máximo estructural detectado
- Nuevo mínimo estructural detectado
