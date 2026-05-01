---
name: strategy-debug
description: >
  Debug snippets para trendlines-strategy.pine. Activar SOLO cuando el usuario diga
  "activa debug", "vamos a hacer debug", "habilita debug", "necesito debugear lineas",
  "agrega contadores de lineas", "muestra test lines" o frases similares de diagnostico.
  Contiene 4 bloques de debug independientes que se inyectan en el codigo a peticion:
  (1) Counters de line.new/line.delete para diagnosticar max_lines_count;
  (2) Test lines visuales con coords hardcoded vs state-vars para verificar render;
  (3) Tabla debug 9-cols con comparacion state vs line.get_*, Y@now, Y@line, delta;
  (4) BK floating debug labels con contadores y distancias HTF.
  El usuario NO debe ver ningun input toggle de debug — solo el dev/Claude activa.
  Cuando se desactive debug, REMOVER todo el codigo (no dejar comentado).
---

# Strategy Debug Skill

Snippets de debug para `trendlines-strategy.pine`. Mantener el archivo limpio en producción; estos bloques solo se inyectan a petición durante diagnóstico.

## Cuándo usar

Cuando el usuario diga:
- "activa debug" / "vamos a hacer debug" / "habilita debug"
- "necesito debugear lineas"
- "agrega contadores de lineas"
- "muestra test lines"
- "no veo las lineas, necesito tabla de debug"

## Cuándo NO usar

- En código de producción / commits finales
- Cuando el usuario hace `ship` o pide "limpia debug" / "remueve debug"
- Cuando solo necesita ver trades del backtest (eso ya está en la lógica de strategy.entry/exit, no requiere este skill)

## Flujo de activación

1. Preguntar al usuario qué bloques quiere activar (puede ser solo 1, 2, 3 o todos)
2. Inyectar el código de cada bloque elegido en las ubicaciones indicadas
3. Verificar que compila
4. Documentar en TodoWrite que el debug está activo
5. Cuando el usuario diga "limpia" o "remueve debug" → eliminar TODO el código inyectado

## Ubicación de inyección en el archivo

```
/Users/andreyalvarado/Documents/tradingStrategy/strategy-777/trendlines-strategy.pine
```

---

## BLOQUE 1 — Line counters (max_lines_count diagnostic)

**Propósito**: Pine v6 limita a 500 líneas vivas. Si se excede, purga las más antiguas silenciosamente. Este bloque cuenta cumulativamente cada `line.new` y `line.delete` para diagnosticar si estamos saturando el pool.

### Declaración (insertar al inicio del archivo, después del `strategy()` block)

```pine
// Contadores debug para max_lines_count (DEV ONLY - no exponer al usuario)
var int _lcN = 0  // total line.new creadas
var int _lcD = 0  // total line.delete ejecutadas
```

### Instrumentación (envolver CADA `line.new(` con `_lcN += 1` antes)

Usar Python o sed para automatizar las 27+ inserciones:

```python
import re
with open('trendlines-strategy.pine', 'r') as f:
    lines = f.readlines()

new_lines = []
for line in lines:
    stripped = line.lstrip()
    indent = line[:len(line) - len(stripped)]
    if stripped.startswith('//'):
        new_lines.append(line)
        continue
    if 'line.new(' in line and '_lcN' not in line:
        new_lines.append(f"{indent}_lcN += 1\n")
        new_lines.append(line)
    elif 'line.delete(' in line and '_lcD' not in line and not stripped.startswith('//'):
        new_lines.append(f"{indent}_lcD += 1\n")
        new_lines.append(line)
    else:
        new_lines.append(line)

with open('trendlines-strategy.pine', 'w') as f:
    f.writelines(new_lines)
```

### Mostrar en chart (label esquina)

```pine
// Label de contadores en bottom-right del chart
var label _lcLbl = na
label.delete(_lcLbl)
int _lcNet = _lcN - _lcD
string _lcTxt = "Cre: " + str.tostring(_lcN) + "  Del: " + str.tostring(_lcD) + "  Net: " + str.tostring(_lcNet) + "  Max: 500"
_lcLbl := label.new(bar_index, low - atrVal * 8, _lcTxt, style=label.style_label_up, color=color.new(color.black, 20), textcolor=_lcNet > 500 ? color.red : color.lime, size=size.normal)
```

### Limpieza

```bash
# Remover instrumentación
python3 << 'PYEOF'
with open('trendlines-strategy.pine', 'r') as f:
    content = f.read()
import re
content = re.sub(r'^\s*_lc[ND] \+= 1\n', '', content, flags=re.MULTILINE)
content = re.sub(r'\n*var int _lcN = 0.*\n', '\n', content)
content = re.sub(r'\n*var int _lcD = 0.*\n', '\n', content)
with open('trendlines-strategy.pine', 'w') as f:
    f.write(content)
PYEOF
```

---

## BLOQUE 2 — Test lines (verificar render de Pine)

**Propósito**: Si las TL no se ven, agregar líneas de prueba con coords conocidas para aislar el problema. CRÍTICO: Pine NO renderea líneas con `x1` muy lejano en el pasado (~>300 bars). Esto se descubrió usando estas test lines.

### Inyectar después del bloque "FORCE RECREATE" (cerca de la sección de sync de labels)

```pine
// =============================================================================
// TEST LINES (DEV DEBUG - eliminar despues de diagnostico)
// =============================================================================

// VERDE: control. Hardcoded horizontal con coords cercanas. Siempre visible.
var line _testLine = na
line.delete(_testLine)
_testLine := line.new(bar_index - 30, low - atrVal * 2, bar_index, low - atrVal * 2,
                       color=color.lime, width=4, extend=extend.right)

// AMARILLO: usa coords reales de _tlH_cur (1B). Si NO se ve pero verde si, b1 es muy lejano.
var line _testLine2 = na
line.delete(_testLine2)
if not na(_b1H_cur) and not na(_b2H_cur)
    _testLine2 := line.new(_b1H_cur, _v1H_cur, _b2H_cur, _v2H_cur,
                            color=color.yellow, width=10, extend=extend.right)

// FUCSIA: misma slope que 1B, pero +50 USD vertical. Descarta z-order issue con candles.
var line _testLine3 = na
line.delete(_testLine3)
if not na(_b1H_cur) and not na(_b2H_cur)
    _testLine3 := line.new(_b1H_cur, _v1H_cur + 50, _b2H_cur, _v2H_cur + 50,
                            color=color.fuchsia, width=10, extend=extend.right)

// NARANJA: misma slope 1B pero anclada en bar_index (no past). Si visible, slope OK.
var line _testLine4 = na
line.delete(_testLine4)
if not na(_b1H_cur) and not na(_b2H_cur) and _b2H_cur > _b1H_cur
    float _slope4 = (_v2H_cur - _v1H_cur) / float(_b2H_cur - _b1H_cur)
    float _yNow4  = _v2H_cur + _slope4 * float(bar_index - _b2H_cur)
    _testLine4 := line.new(bar_index - 50, _yNow4 - _slope4 * 50, bar_index, _yNow4,
                            color=color.orange, width=10, extend=extend.right)

// MAGENTA: hardcoded x1 lejano (-400 bars). Confirma si Pine no renderea con x1 lejos.
var line _testLine5 = na
line.delete(_testLine5)
_testLine5 := line.new(bar_index - 400, low - atrVal * 4, bar_index - 50, low - atrVal * 3,
                        color=#FF00FF, width=10, extend=extend.right)

// LABEL DE VALORES REALES de state vars
var label _dbgB1Lbl = na
label.delete(_dbgB1Lbl)
string _s1 = "_b1H_cur=" + (na(_b1H_cur) ? "NA" : str.tostring(_b1H_cur))
string _s2 = "_v1H_cur=" + (na(_v1H_cur) ? "NA" : str.tostring(_v1H_cur, format.mintick))
string _s3 = "_b2H_cur=" + (na(_b2H_cur) ? "NA" : str.tostring(_b2H_cur))
string _s4 = "_v2H_cur=" + (na(_v2H_cur) ? "NA" : str.tostring(_v2H_cur, format.mintick))
string _s5 = "bar_index=" + str.tostring(bar_index)
string _dbgB1Txt = _s1 + "\n" + _s2 + "\n" + _s3 + "\n" + _s4 + "\n" + _s5
_dbgB1Lbl := label.new(bar_index, low - atrVal * 6, _dbgB1Txt, style=label.style_label_up, color=color.new(color.black, 20), textcolor=color.lime, size=size.normal)
```

### Interpretación

| Caso | Conclusión |
|------|-----------|
| Verde se ve, las demás no | Pine sí renderea, pero coords/x1 lejano son problema |
| Verde + Naranja se ven, Amarillo/Fucsia no | x1 muy lejano = bug confirmado, fix: anclar en `bar_index - N` |
| Magenta no se ve (x1=-400) | Confirma límite de Pine para x1 lejano |
| Fucsia se ve pero amarillo no | z-order: candles cubren la línea al mismo Y |

---

## BLOQUE 3 — Tabla debug 9-columnas

**Propósito**: Comparar state vars (`_b2H_cur`, `_v2H_cur`) con coords reales de la línea (`line.get_x2`, `line.get_y2`) y `Y@now` (proyección) vs `Y@line` (`line.get_price`). Δ debe ser 0 si todo está sincronizado.

**Limitación**: `line.get_price()` solo funciona con líneas creadas con `xloc=xloc.bar_index`. Para HTF (que usan `xloc.bar_time`), Y@line no aplica.

### Inyectar después del bloque "Per-bar position updates"

```pine
// Tabla debug 9-cols: estado lineas (DEV ONLY)
var table _dbgTable = na

if i_backtesting  // o usar un flag interno tipo `var bool _dbgEnable = true`
    if na(_dbgTable)
        _dbgTable := table.new(position.bottom_right, 9, 13,
                               bgcolor=color.new(color.black, 20),
                               border_width=1, border_color=color.new(color.gray, 50))
        // Headers
        table.cell(_dbgTable, 0, 0, "LINE",   text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 1, 0, "B2st",   text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 2, 0, "X2ln",   text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 3, 0, "V2st",   text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 4, 0, "Y2ln",   text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 5, 0, "Sc",     text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 6, 0, "Y@now",  text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 7, 0, "Y@line", text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)
        table.cell(_dbgTable, 8, 0, "Δ",      text_color=color.yellow, bgcolor=color.new(color.gray, 70), text_size=size.small)

    // Helpers para cada linea
    int   _ln_x2_1  = na(_tlH_cur)  ? int(na)   : line.get_x2(_tlH_cur)
    float _ln_y2_1  = na(_tlH_cur)  ? float(na) : line.get_y2(_tlH_cur)
    float _ln_pr_1  = na(_tlH_cur)  ? float(na) : line.get_price(_tlH_cur, bar_index)
    // ... repetir para _tlH_prv, _tlH_old, _tlL_cur, _tlL_prv, _tlL_old, _tlBK_cur

    // Fila 1B (H cur)
    string _d1 = na(_curH_now) or na(_ln_pr_1) ? "-" : str.tostring(_curH_now - _ln_pr_1, format.mintick)
    color _dc1 = na(_curH_now) or na(_ln_pr_1) ? color.gray : math.abs(_curH_now - _ln_pr_1) < 0.5 ? color.lime : color.red
    table.cell(_dbgTable, 0, 1, "1B (H cur)", text_color=color.white, text_size=size.small)
    table.cell(_dbgTable, 1, 1, na(_b2H_cur) ? "-" : str.tostring(_b2H_cur), text_color=color.white, text_size=size.small)
    table.cell(_dbgTable, 2, 1, na(_ln_x2_1) ? "-" : str.tostring(_ln_x2_1), text_color=na(_b2H_cur) or na(_ln_x2_1) or _b2H_cur == _ln_x2_1 ? color.white : color.red, text_size=size.small)
    table.cell(_dbgTable, 3, 1, na(_v2H_cur) ? "-" : str.tostring(_v2H_cur, format.mintick), text_color=color.white, text_size=size.small)
    table.cell(_dbgTable, 4, 1, na(_ln_y2_1) ? "-" : str.tostring(_ln_y2_1, format.mintick), text_color=color.white, text_size=size.small)
    table.cell(_dbgTable, 5, 1, str.tostring(_scH_cur), text_color=color.white, text_size=size.small)
    table.cell(_dbgTable, 6, 1, na(_curH_now) ? "-" : str.tostring(_curH_now, format.mintick), text_color=color.aqua, text_size=size.small)
    table.cell(_dbgTable, 7, 1, na(_ln_pr_1) ? "-" : str.tostring(_ln_pr_1, format.mintick), text_color=color.aqua, text_size=size.small)
    table.cell(_dbgTable, 8, 1, _d1, text_color=_dc1, text_size=size.small)
    // ... repetir para todas las filas (2B, 3B, 1A, 2A, 3A, BK, HTF1B, HTF1A, HTF4B, HTF4A)
```

> [!note] Para el set completo de 11 filas con HTF, reconstruir manualmente expandiendo el patrón. Las HTF NO usan `line.get_price` (incompatible con `xloc.bar_time`).

---

## BLOQUE 4 — BK floating debug labels

**Propósito**: Labels flotantes encima/debajo del precio mostrando contadores de break, distancias a TL, scores y razones por las que un BK no fired. Útil para debuggear lógica de break confirmation.

### Inyectar al final del bloque de cálculos BK

```pine
// BK debug labels (DEV ONLY)
var label _dbgH = na
var label _dbgL = na

string _dH = "H cntCur=" + str.tostring(_bkH_cnt_cur) +
             " cntPrv=" + str.tostring(_bkH_cnt_prv) +
             "\nscCur=" + str.tostring(_scH_cur) +
             " scPrv=" + str.tostring(_scH_prv) +
             "\nclsCur=" + (not na(_curH_now) ? str.tostring(close - _curH_now, format.mintick) : "na") +
             " clsPrv=" + (not na(_prvH_now) ? str.tostring(close - _prvH_now, format.mintick) : "na") +
             "\ndist=" + str.tostring(_bkMinDist, format.mintick) +
             "\nb2c=" + str.tostring(_b2H_cur) +
             " b2p=" + str.tostring(_b2H_prv) +
             "\n--- HTF ---" +
             "\nH1B=" + (not na(htf1H_now) ? str.tostring(htf1H_now, format.mintick) : "na") +
             " (d=" + (not na(htf1H_now) ? str.tostring(htf1H_now - close, format.mintick) : "na") + ")" +
             "\nH4B=" + (not na(htf2H_now) ? str.tostring(htf2H_now, format.mintick) : "na") +
             " (d=" + (not na(htf2H_now) ? str.tostring(htf2H_now - close, format.mintick) : "na") + ")"

if na(_dbgH)
    _dbgH := label.new(bar_index, high + atrVal * 3, _dH, style=label.style_label_down, color=color.new(color.blue, 70), textcolor=color.white, size=size.normal)
else
    label.set_x(_dbgH, bar_index)
    label.set_y(_dbgH, high + atrVal * 3)
    label.set_text(_dbgH, _dH)

string _dL = "L cntCur=" + str.tostring(_bkL_cnt_cur) +
             " cntPrv=" + str.tostring(_bkL_cnt_prv) +
             "\nscCur=" + str.tostring(_scL_cur) +
             " scPrv=" + str.tostring(_scL_prv) +
             "\nclsCur=" + (not na(_curL_now) ? str.tostring(close - _curL_now, format.mintick) : "na") +
             " clsPrv=" + (not na(_prvL_now) ? str.tostring(close - _prvL_now, format.mintick) : "na") +
             "\ndist=" + str.tostring(_bkMinDist, format.mintick) +
             "\nb2c=" + str.tostring(_b2L_cur) +
             " b2p=" + str.tostring(_b2L_prv) +
             "\n--- HTF ---" +
             "\nH1A=" + (not na(htf1L_now) ? str.tostring(htf1L_now, format.mintick) : "na") +
             " (d=" + (not na(htf1L_now) ? str.tostring(close - htf1L_now, format.mintick) : "na") + ")" +
             "\nH4A=" + (not na(htf2L_now) ? str.tostring(htf2L_now, format.mintick) : "na") +
             " (d=" + (not na(htf2L_now) ? str.tostring(close - htf2L_now, format.mintick) : "na") + ")"

if na(_dbgL)
    _dbgL := label.new(bar_index, low - atrVal * 3, _dL, style=label.style_label_up, color=color.new(color.purple, 70), textcolor=color.white, size=size.normal)
else
    label.set_x(_dbgL, bar_index)
    label.set_y(_dbgL, low - atrVal * 3)
    label.set_text(_dbgL, _dL)
```

---

## Workflow completo

### Activar (cuando user dice "activa debug")

1. Preguntar al usuario qué bloques (1-4 o todos)
2. Para cada bloque elegido:
   - Leer la sección correspondiente de este SKILL
   - Inyectar el código en la ubicación indicada usando Edit tool
3. Verificar compila (no hay error syntax)
4. Marcar en TodoWrite: "Debug activo: bloques [X, Y, Z]"

### Limpiar (cuando user dice "limpia debug" o "remueve debug")

1. Para cada bloque inyectado:
   - Encontrar el bloque por sus marcadores únicos (`_lcN`, `_testLine`, `_dbgTable`, `_dbgH`)
   - Eliminar todas las apariciones (declaraciones, instrumentaciones, labels)
2. Verificar el archivo compila limpio
3. Hacer un git diff para asegurar que no quedó nada residual

### Lecciones aprendidas durante diagnóstico (sesión Mayo 1, 2026)

1. **Pine NO renderea líneas con `x1` muy lejano** (~>300 bars en el pasado). Las líneas con coords de pivotes lejanos quedan invisibles aunque `line.get_x1/y1` devuelvan valores correctos.

2. **Fix definitivo**: anclar líneas en `bar_index - N` (típicamente N=100) preservando la slope original calculada de `(v2-v1)/(b2-b1)`. Las HTF (xloc.bar_time) no sufren este bug.

3. **`line.set_xy*` falla silenciosamente en handles purgados**. Si max_lines_count se excede y la línea es purgada, `line.set_xy1/xy2` no hace nada visible. Solución: usar `line.delete + line.new` cada bar para garantizar handle vivo.

4. **`line.get_x1` no devuelve `na` para líneas purgadas** — Pine quirk. La detección de purge vía `na(line.get_x1(handle))` no es confiable.

5. **Cumulative count vs alive count**: `_lcN` cuenta CUMULATIVAMENTE en toda la historia del backtest. Lo que importa para visibilidad es que las 7-11 líneas activas estén entre las últimas 500 creadas.
