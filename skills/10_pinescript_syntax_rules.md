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

## Referencia de errores comunes

| Error | Causa probable |
|---|---|
| `Mismatched input 'end of line without line continuation'` | strategy() con ( solo en su linea, o caracter non-ASCII en el archivo |
| `Undeclared identifier` | Variable usada antes de ser declarada |
| `Cannot call 'strategy.entry' in local scope` | strategy.entry() dentro de una funcion |
| `Loop is too long` | for loop iterando demasiados elementos en tiempo real |
