---
tags: [estrategia, pine-script, trendlines, htf, xauusd, sesiones]
archivo: trendlines-strategy.pine
version: pine-v6
updated: 2026-04-29
---

# Trendlines Strategy

Estrategia overlay Pine Script v6 para XAUUSD en 5m. Detecta rechazos de price action contra trendlines de temporalidades superiores (H1 y H4) durante sesiones operativas (LD y NY). Gestión de riesgo fija en USD.

---

## Arquitectura

```
HTF1 (H1) ─┐
            ├─► Proyección al bar de 5m ─► Detección de rechazo ─► Entrada + SL/TP
HTF2 (H4) ─┘
```

- **Ejecución**: 5m (señal + entry + exit)
- **Niveles**: trendlines proyectadas de H1 y H4 por interpolación temporal
- **Filtro de sesión**: LD (07–12 UTC) y NY (12–17 UTC)
- **Riesgo fijo**: $60 por operación (configurable)
- **TP**: 1:2 default, 1:3 opcional

---

## Parámetros Clave

| Grupo | Parámetro | Default | Rango útil |
|-------|-----------|---------|------------|
| Sesiones | Asia/LD/NY UTC | 0/7/12 | Ajustar para DST |
| HTF1 | Timeframe | 60 (H1) | 30–240 |
| HTF1 | Periodo pivote | 5 | 3–10 |
| HTF2 | Timeframe | 240 (H4) | 60–D |
| HTF2 | Periodo pivote | 5 | 3–10 |
| Señales | Wick ratio | 0.25 | 0.15–0.45 |
| Señales | Tolerancia ATR | 0.5× | 0.3–0.8 |
| Señales | SL buffer ATR | 1.2× | 0.8–2.0 |
| Señales | Cooldown | 5 barras | 3–15 |
| Riesgo | USD por trade | 60 | libre |

---

## Lógica de Entrada

### Rechazo bajista (SHORT)
```
high ∈ [TL − tol, TL + tol×1.5]
AND close[1] < TL − tol×0.4
AND wick_superior / rango >= wickRatio
AND sesión activa
AND sin posición abierta
AND cooldown cumplido
```

### Rechazo alcista (LONG)
```
low ∈ [TL − tol×1.5, TL + tol]
AND close[1] > TL + tol×0.4
AND wick_inferior / rango >= wickRatio
AND sesión activa
AND sin posición abierta
AND cooldown cumplido
```

### Prioridad y probabilidad

| Fuente | Peso | Score con LD | Score con NY | Label |
|--------|------|-------------|-------------|-------|
| HTF2 + LD + NY | 4 | — | — | HTF2+Ses: MUY ALTA |
| HTF2 + sesión | 3 | ✓ | ✓ | ALTA |
| HTF1 + sesión | 2 | ✓ | ✓ | MEDIA-ALTA |
| HTF1 solo | 1 | — | — | MEDIA |

HTF2 tiene prioridad sobre HTF1 en la misma dirección (sin señales duplicadas).

---

## Visualización

- **Cajas de sesión**: Asia (blanco), LD (lila), NY (cian) — bounded por H/L real acumulado
- **Líneas HTF2**: sólidas, grosor 3, violeta `#9B59B6` (resistencia) / blanco `#FFFFFF` (soporte)
- **Líneas HTF1**: dashed, grosor 2, lila claro `#CE93D8` / blanco `#FFFFFF`
- **Líneas TF actual**: dotted, solo visual, sin señales
- **Labels de entrada**: incluyen fuente, sesión, E/SL, riesgo y profit estimado
- **Líneas SL/TP**: horizontales desde la entrada

---

## Skills Relacionados

### Antes de operar — Contexto macro

- [[forex-gold/tradermonty/market-environment-analysis]] — DXY, yields, spread EM, commodities: valida si el sesgo macro apoya la dirección del trade
- [[forex-gold/tradermonty/macro-regime-detector]] — identifica régimen actual (inflacionario/deflacionario): en bull gold los longs HTF tienen mayor probabilidad
- [[forex-gold/tradermonty/market-news-analyst]] — FOMC, NFP, CPI, geopolítica: evitar entrar antes de noticias de alto impacto

### Análisis de chart HTF

- [[forex-gold/tradermonty/technical-analyst]] — pasa screenshot del H4 o H1 de XAUUSD para validar dirección de las trendlines generadas por la estrategia antes de confiar en ellas

### Backtesting externo

- [[crypto-trading/trading-strategy-backtester]] — backtest en Python con datos históricos de XAUUSD para validar parámetros fuera de TradingView (Sharpe, drawdown, win rate)
- [[forex-gold/tradermonty/backtest-expert]] — revisión metodológica del backtest y validación de métricas

### Riesgo y sizing

- [[forex-gold/tradermonty/position-sizer]] — valida que el cálculo `qty = riskUSD / slDist` sea correcto para el broker y el par (spreads, pip value, margen)

### Planificación de sesión

- [[forex-gold/tradermonty/scenario-analyzer]] — antes de LD: define escenarios bull/bear/neutral y los targets donde las trendlines podrían ser tocadas
- [[forex-gold/tradermonty/economic-calendar-fetcher]] — obtiene el calendario del día para filtrar ventanas de alta volatilidad (evitar señales 15 min antes/después de noticias)

### Flujo institucional (contexto de soporte)

- [[forex-gold/tradermonty/institutional-flow-tracker]] — verifica si el smart money está posicionado en la misma dirección que el rechazo detectado
- [[crypto-trading/options-flow-analyzer]] — flujo de opciones sobre oro (GLD, micro-futures) como confirmación de dirección

### Análisis continuo

- [[ai-ml/anomaly-detection-system]] — detecta si el comportamiento del precio en el rechazo es atípico respecto al histórico (puede señalar un falso setup)
- [[ai-ml/time-series-forecaster]] — forecast de precio a corto plazo para estimar si el TP es alcanzable en la sesión actual

---

## Workflow Operativo

```
1. [macro-regime-detector]      → ¿régimen favorece la dirección?
2. [economic-calendar-fetcher]  → ¿hay noticias en la próxima hora?
3. [technical-analyst]          → ¿las TL en H4/H1 están bien dibujadas?
4. TradingView 5m               → esperar señal de rechazo en la estrategia
5. [scenario-analyzer]          → ¿el trade encaja en el escenario planificado?
6. Entrada ejecutada             → registrar en journal
7. Post-sesión [backtest-expert] → revisar si los parámetros siguen siendo óptimos
```

---

## Archivos

| Archivo | Descripción |
|---------|-------------|
| [[../indicators/trendlines]] | Indicador original de trendlines (solo visual, sin señales) |
| `trendlines-strategy.pine` | Esta estrategia completa |
| [[../reference/acceptanceCriteria]] | Criterios de aceptación del proyecto |
| [[../reference/pineScriptSyntax]] | Reglas de sintaxis Pine Script v6 |
| [[../reference/backtesting]] | Guía de backtesting del proyecto |
