---
tags: [bugs, pendientes, pandora]
updated: 2026-04-26
---

# Pandora — Bugs y Mejoras Pendientes

> [!quote] Caja de Pandora
> Cuando el usuario diga **"abre pandora"** → revisar esta lista y priorizar con el usuario.

---

## BUG-001 — Fake Signals

> [!bug] Fake Signals: setup válido pero mercado va en dirección contraria
> **Archivo:** `acc-scalping.pine` · **Estado:** ⏳ Pendiente · **Fecha:** 2026-04-17
>
> La estrategia genera setups técnicamente correctos pero el mercado termina yendo en dirección contraria. El setup ocurre en **contratendencia** respecto al contexto macro — el mercado está en sellers y el setup de compra es solo un pullback o trampa.

### Opciones

| Opción | Viabilidad |
|--------|-----------|
| HTF trend filter — solo longs si HTF close > HTF EMA(N) | Alta |
| HTF estructura de mercado — solo longs si HTF hace higher lows | Media |
| Volumen relativo — solo entrar si volumen > SMA(vol, 20) | Media |
| Order book % buys/sells | No disponible en TradingView |

### Propuesta Recomendada

> [!tip] HTF Trend Filter — implementación sugerida
> ```pine
> i_useHTF       = input.bool(false, "Use HTF Filter")
> i_htfTF        = input.timeframe("60", "HTF Timeframe")
> i_htfEmaPeriod = input.int(20, "HTF EMA Period")
>
> htfClose = request.security(syminfo.tickerid, i_htfTF, close)
> htfEma   = request.security(syminfo.tickerid, i_htfTF, ta.ema(close, i_htfEmaPeriod))
>
> bullEntry = bullSetup[1] and ... and (not i_useHTF or htfClose > htfEma)
> bearEntry = bearSetup[1] and ... and (not i_useHTF or htfClose < htfEma)
> ```
> Debe ser activable para comparar backtesting con y sin el filtro.

---

## BUG-002 — Tabla RR

> [!bug] Tabla RR recomienda ratio que no coincide con backtesting real
> **Archivo:** `acc-scalping.pine` — sección `RR RECOMMENDATION TABLE` · **Estado:** ⏳ Pendiente · **Fecha:** 2026-04-17
>
> La tabla muestra "Best RR: 1:3" pero el informe de TradingView reporta RR real ~1:1.8.

### Causa

> [!warning] Cálculo incorrecto
> El EV usa RRs teóricos fijos sin considerar que algunos trades tienen el TP capado por `i_maxTPDist`. La tabla cuenta ese trade como "win completo" pero dio menos de 2R.
> ```pine
> ev12 = wr * 2.0 - (1.0 - wr) * 1.0  // asume que todo win da exactamente 2R ← incorrecto
> ```

### Corrección Propuesta
Calcular RR real usando `strategy.closedtrades.profit()` comparado contra la distancia real de SL, o mostrar el RR promedio efectivo observado.

> [!note] Impacto
> Solo afecta display/análisis — no afecta entradas ni salidas.

---

## Agregar un Bug

```
## BUG-XXX — Título

> [!bug] Descripción corta
> **Archivo:** · **Estado:** · **Fecha:**
> Descripción detallada.
```
