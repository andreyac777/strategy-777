# Reglas de Sintaxis y Encoding para Pine Script

## Proposito
Este documento contiene reglas obligatorias para evitar errores de parsing en Pine Script.
Deben aplicarse en cada archivo .pine generado, sin excepcion.

---

## REGLA 1 — //@version=5 debe ser la linea 1 absoluta

La anotacion de version DEBE ser la primera linea del archivo, antes de cualquier comentario.

### Correcto
```
//@version=5
// Comentario del autor
strategy(...)
```

### Incorrecto
```
// Comentario del autor
//@version=5
strategy(...)
```

---

## REGLA 2 — strategy() debe tener el primer parametro en la misma linea

Pine Script es sensible al formato de llamadas multi-linea. La forma mas estable es
poner el primer argumento en la misma linea que el parentesis de apertura.

### Correcto
```pine
strategy(title="Nombre",
     shorttitle="CORTO",
     overlay=true,
     initial_capital=10000,
     default_qty_type=strategy.percent_of_equity,
     default_qty_value=1,
     commission_type=strategy.commission.percent,
     commission_value=0.05,
     slippage=2,
     max_bars_back=500,
     pyramiding=0)
```

### Incorrecto (puede causar "end of line without line continuation")
```pine
strategy(
    title = "Nombre",
    ...
)
```

---

## REGLA 3 — Solo ASCII en todo el archivo

Pine Script falla silenciosamente con caracteres no-ASCII, incluso dentro de comentarios.

### Caracteres PROHIBIDOS
- Em dash: — (U+2014) → usar -
- En dash: – (U+2013) → usar -
- Acentos: á é í ó ú ü → usar a e i o u u
- Enye: ñ → usar n
- Comillas tipograficas: " " ' ' → usar " y '
- Cualquier otro caracter fuera del rango ASCII (0x00–0x7F)

### Regla de oro
Si el caracter no aparece en un teclado US estandar, no debe estar en el archivo .pine.

### Palabras afectadas comunes en este proyecto
| Original | Reemplazar por |
|---|---|
| senales | senales |
| modulos | modulos |
| calculo | calculo |
| logica | logica |
| tendencia | tendencia |
| proxima | proxima |
| patron | patron |
| gestion | gestion |
| riesgo/beneficio | riesgo/beneficio |
| visualizacion | visualizacion |

---

## REGLA 4 — No comentarios dentro de expresiones multi-linea

Los comentarios // dentro de parentesis abren o cierran expresiones de forma inesperada.

### Incorrecto
```pine
final_sig = condition_a and (
    sig_long
    // or sig_long_2   <- ROMPE EL PARSER
)
```

### Correcto
```pine
// Para agregar modulo 2: agregar "or s2_sig_long" al final de la linea
final_sig = condition_a and sig_long
```

---

## REGLA 5 — Verificacion antes de entregar cualquier archivo .pine

Antes de dar por terminado un archivo Pine Script, verificar:

- [ ] //@version=5 es la linea 1
- [ ] strategy() tiene el primer parametro en la misma linea
- [ ] Cero caracteres non-ASCII (buscar con: `LC_ALL=C grep -n '[^ -~]' archivo.pine`)
- [ ] Cero comentarios // dentro de parentesis en expresiones multi-linea
- [ ] Cero em dashes (—) ni en dashes (–)
- [ ] Strings de TradingView (title, shorttitle, group names) usan solo ASCII

---

---

## REGLA 6 — Siempre guard array.size() > 0 antes de for i = 0 to array.size() - 1

En Pine Script, `for i = 0 to -1` NO se salta automaticamente cuando el array esta vacio.
Intentara ejecutar con i=0 y `array.get()` lanzara "Index 0 is out of bounds".

### Incorrecto
```pine
for i = 0 to array.size(mi_array) - 1
    val = array.get(mi_array, i)   // CRASH en bar 0 si array esta vacio
```

### Correcto
```pine
if array.size(mi_array) > 0
    for i = 0 to array.size(mi_array) - 1
        val = array.get(mi_array, i)
```

### Aplica a
- Loops de invalidacion de zonas
- Loops de deteccion de senales
- Loops de visualizacion de zonas
- Cualquier `for i = 0 to array.size(arr) - 1` sobre un array que puede estar vacio en bar 0

---

---

## REGLA 7 — Al renombrar una variable, actualizar TODAS sus referencias en el archivo

Pine Script declara variables en orden de ejecucion top-to-bottom. Si una variable se
renombra en un bloque, todas las secciones posteriores que la usen seguiran referenciando
el nombre anterior y lanzaran "Undeclared identifier".

### Patron peligroso: split de variable por direccion
Cuando se divide `_qty` en `_qty_long` y `_qty_short`, la seccion visual que usaba
`_qty` sigue esperando el nombre original.

### Correcto: resolver localmente en el scope que lo necesita
```pine
// Seccion de ejecucion (global)
_qty_long  = math.max(_risk_usd / (_sl_dist_long  * _pv), syminfo.mintick)
_qty_short = math.max(_risk_usd / (_sl_dist_short * _pv), syminfo.mintick)

// Seccion visual (bloque local if)
if i_show_rr and (final_sig_long or final_sig_short)
    _qty = final_sig_long ? _qty_long : _qty_short  // resuelto aqui, no en global
    _tp_amt = str.tostring(math.abs(_tp - _ep) * _qty, "#.##")
```

### Checklist al renombrar variables
- [ ] Buscar TODAS las ocurrencias con grep: `grep -n "_nombre_viejo" archivo.pine`
- [ ] Verificar secciones visuales (R/R boxes, labels, debug)
- [ ] Verificar seccion de debug labels
- [ ] Verificar seccion de tabla analyzer
- [ ] No asumir que solo la seccion editada usa la variable

---

## Referencia de errores comunes

| Error | Causa probable |
|---|---|
| `Mismatched input 'end of line without line continuation'` | strategy() con ( solo en su linea, o caracter non-ASCII en el archivo |
| `Undeclared identifier '_nombre'` | Variable renombrada o refactorizada pero referencia antigua quedo en otra seccion del archivo |
| `Undeclared identifier` | Variable usada antes de ser declarada |
| `Cannot call 'strategy.entry' in local scope` | strategy.entry() dentro de una funcion |
| `Loop is too long` | for loop iterando demasiados elementos en tiempo real |
