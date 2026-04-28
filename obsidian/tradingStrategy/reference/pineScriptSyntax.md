---
tags: [referencia, pine-script, sintaxis, reglas]
updated: 2026-04-26
---

# Pine Script — Reglas de Sintaxis y Encoding

> [!important] Obligatorio
> Aplicar en **cada archivo `.pine`** generado, sin excepción. Verificar antes de entregar.

---

## REGLA 1 — `//@version=5` debe ser la línea 1 absoluta

> [!success] Correcto
> ```pine
> //@version=5
> // Comentario
> strategy(...)
> ```

> [!danger] Incorrecto
> ```pine
> // Comentario
> //@version=5
> ```

---

## REGLA 2 — `strategy()` con primer parámetro en la misma línea

> [!success] Correcto
> ```pine
> strategy(title="Nombre",
>      overlay=true,
>      initial_capital=10000)
> ```

> [!danger] Incorrecto — puede causar `end of line without line continuation`
> ```pine
> strategy(
>     title = "Nombre"
> )
> ```

---

## REGLA 3 — Solo ASCII en todo el archivo

> [!danger] Caracteres prohibidos
> | Prohibido | Reemplazar por |
> |-----------|----------------|
> | `—` (em dash) | `-` |
> | `–` (en dash) | `-` |
> | `á é í ó ú ü` | `a e i o u u` |
> | `ñ` | `n` |
> | `" " ' '` | `"` y `'` |

> [!warning] Nombres de variables — no usar acentos ni ñ
> | Incorrecto | Correcto |
> |------------|----------|
> | `señalFinal` | `signalFinal` |
> | `períodoBars` | `periodoBars` |
> | `cálculo` | `calculo` |

**Verificar con:**
```bash
LC_ALL=C grep -n '[^ -~]' archivo.pine
```

---

## REGLA 4 — No comentarios `//` dentro de expresiones multi-línea

> [!danger] Incorrecto — rompe el parser
> ```pine
> final_sig = condition_a and (
>     sig_long
>     // or sig_long_2
> )
> ```

> [!success] Correcto
> ```pine
> // Para agregar módulo 2: agregar "or s2_sig_long" al final
> final_sig = condition_a and sig_long
> ```

---

## REGLA 5 — Guard `array.size() > 0` antes de for loops

> [!danger] Incorrecto — crash en bar 0 si array está vacío
> ```pine
> for i = 0 to array.size(mi_array) - 1
>     val = array.get(mi_array, i)
> ```

> [!success] Correcto
> ```pine
> if array.size(mi_array) > 0
>     for i = 0 to array.size(mi_array) - 1
>         val = array.get(mi_array, i)
> ```

---

## REGLA 6 — Fixture permanente en bloque visual

> [!important] Esta línea SIEMPRE debe existir en el bloque visual
> ```pine
> _qty = final_sig_long ? _qty_long : _qty_short
> ```
> **Verificar:**
> ```bash
> grep -n "_qty" archivo.pine | grep "final_sig_long"
> ```

---

## REGLA 7 — Confirmar antes de tocar código compartido

> [!warning] Secciones que requieren confirmación explícita
> Signal Aggregation · Trade Execution · Visual Engine · Risk · Session · Debug · Analyzer Table
>
> Antes de editar fuera del bloque del módulo pedido → **preguntar al usuario**.

---

## REGLA 8 — Asignar siempre el retorno de `label.new()` y `line.new()`

> [!danger] Incorrecto — CE10144 "missing a local code block"
> ```pine
> if condition
>     line.new(...)
>     label.new(...)
>
> if next_condition   // error: Pine ve el if anterior como expresion sin cerrar
>     ...
> ```

> [!success] Correcto
> ```pine
> if condition
>     _sl = line.new(...)
>     _lb = label.new(...)
>
> if next_condition   // ok
>     ...
> ```

> [!warning] Por que ocurre
> Pine Script trata el ultimo statement de un bloque `if` como el valor de retorno del bloque.
> Si ese valor es un objeto (`label`, `line`, `box`), el parser deja el bloque "abierto"
> y el siguiente `if` al mismo nivel queda sin cuerpo.
> La asignacion a una variable local cierra el bloque explicitamente.

**Verificar con:**
```bash
grep -n "label.new\|line.new\|box.new" archivo.pine | grep -v "^\s*[a-z_]*\s*="
```

---

## Checklist de Entrega

> [!todo] Verificar antes de cada commit
> - [ ] `//@version=5` es la línea 1
> - [ ] `strategy()` tiene primer param en misma línea
> - [ ] Cero non-ASCII
> - [ ] Cero `//` dentro de paréntesis en expresiones multi-línea
> - [ ] Cero em dashes / en dashes
> - [ ] Fixture `_qty` intacta en bloque visual

---

## REGLA 9 — Límites máximos de `strategy()`

> [!danger] CE10178 — valor mayor al máximo permitido
> ```pine
> // Incorrecto
> strategy("...", max_lines_count=1000, max_boxes_count=1000, max_labels_count=1000)
> ```

> [!success] Correcto — topes máximos en Pine Script v6
> ```pine
> strategy("...", max_lines_count=500, max_boxes_count=500, max_labels_count=500)
> ```

> [!warning] Límites fijos (no configurables)
> | Parámetro | Máximo |
> |-----------|--------|
> | `max_lines_count` | 500 |
> | `max_boxes_count` | 500 |
> | `max_labels_count` | 500 |

---

## Referencia de Errores

| Error | Causa probable |
|-------|----------------|
| `Mismatched input 'end of line without line continuation'` | `strategy()` con `(` solo en su línea, o non-ASCII |
| `Undeclared identifier '_nombre'` | Variable renombrada pero referencia antigua en otra sección |
| `Cannot call 'strategy.entry' in local scope` | `strategy.entry()` dentro de una función |
| `Loop is too long` | For loop con demasiados elementos en tiempo real |
| `max_lines_count value (N) is greater than the maximum possible value (500)` (CE10178) | `max_lines_count`, `max_boxes_count` o `max_labels_count` con valor > 500 |
