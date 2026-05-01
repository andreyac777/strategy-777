---
name: bitacora
description: >
  Registrar entrada en bitacora.md cuando el usuario diga "bitacora", "hagamos bitacora",
  "anota bitacora" o frases similares. Cada entrada vincula un commit con metricas
  reales del backtest (profit, drawdown, trades, win rate) para tener un historial
  evolutivo de la estrategia. CRITICO: antes de registrar, verificar que el usuario
  haya hecho ship con Claude Code. Si no se confirma explicitamente "ya hice ship"
  o equivalente, preguntar primero. El usuario adjunta una imagen del informe de
  TradingView con las metricas. Esta skill toma el ultimo commit, extrae datos de la
  imagen, agrega una fila a la tabla de bitacora.md, y muestra un resumen de progreso.
---

# Strategy Bitacora Skill

Skill para llevar un historial evolutivo de la estrategia: cada commit ↔ metricas reales del backtest.

## Cuando usar

Cuando el usuario diga:
- "bitacora"
- "hagamos bitacora"
- "anota bitacora"
- "registra en bitacora"
- "vamos a hacer bitacora"

## Pre-condiciones obligatorias

ANTES de hacer cualquier cosa:

1. **El usuario debe haber hecho `ship` con Claude Code primero**. Si no confirma explicitamente con "ya hice ship", "ya shippeé", "ya commiteé" o similar, PREGUNTAR:
   > "¿Ya hiciste el ship con Claude Code? Necesito que el commit este registrado para tomar el ID."

2. **El usuario debe adjuntar una imagen** del informe de estrategia de TradingView con las metricas. Si no hay imagen, PREGUNTAR:
   > "¿Me puedes mandar el screenshot del informe de estrategia con las metricas (trades, profit, drawdown, win rate)?"

3. NO PROCEDER hasta tener ambas cosas (ship confirmado + imagen).

## Ubicacion del archivo

```
/Users/andreyalvarado/Documents/tradingStrategy/strategy-777/obsidian/tradingStrategy/strategy/bitacora.md
```

Si el archivo no existe aun, crearlo con el header de tabla.

## Workflow

### Paso 1: Verificar pre-condiciones
Confirmar ship + imagen presente. Si falta algo, preguntar y esperar.

### Paso 2: Obtener ultimo commit
```bash
cd /sessions/busy-epic-turing/mnt/tradingStrategy/strategy-777/
git log -1 --pretty=format:"%h | %s | %ad" --date=short
```

Esto da: commit-hash | mensaje | fecha (ej: `4de4717 | feat(trendlines-strategy): timezone local | 2026-05-01`)

### Paso 3: Extraer datos de la imagen
La imagen mostrara metricas similares a:
- **Ganancias y perdidas totales**: ej. `+6.028,16 USD (+0,60%)`
- **Perdida max del capital**: ej. `3.793,14 USD (0,38%)`
- **Total de operaciones**: ej. `188`
- **Operaciones rentables**: ej. `38,83% (73/188)`
- **Factor de ganancia**: ej. `1,20`

Usa vision para leer estos numeros directamente del screenshot. Si alguno no es legible, preguntar al usuario.

### Paso 4: Generar resumen del commit
Resumen de 1-2 lineas con lo principal que se implemento en ese commit. Tomar contexto del:
- Mensaje del commit
- Cambios recientes en la sesion actual de Claude
- Archivos modificados (`git show --stat HEAD`)

Ejemplo: "Refactor multi-tier: 3 niveles independientes (macro/medium/micro), BK solo en 1A/1B, cortes visuales por interseccion."

### Paso 5: Calcular delta vs entrada anterior
Si hay entrada previa en bitacora.md, calcular:
- Δ profit USD (positivo = mejora, negativo = empeora)
- Δ profit %
- Δ win rate
- Δ profit factor

### Paso 6: Append entrada a la tabla
Agregar fila al final de la tabla en `bitacora.md` con este formato:

```markdown
| Fecha | Commit | Cambio | Trades | Profit USD | Profit % | Win % | DD % | PF | Δ vs anterior |
|---|---|---|---|---|---|---|---|---|---|
| 2026-05-01 | 4de4717 | timezone local + purge detection | 188 | +6,028.16 | +0.60% | 38.83% | 0.38% | 1.20 | — (primera entrada) |
| 2026-05-02 | abc1234 | refactor multi-tier | 152 | +7,250.00 | +0.72% | 42.10% | 0.31% | 1.45 | +1,221.84 USD ↑ |
```

### Paso 7: Mostrar resumen de progreso
Despues de guardar, mostrar al usuario:
- Confirmar entrada agregada con commit ID
- Resumen de progreso evolutivo:
  - Numero total de entradas en la bitacora
  - Profit acumulado (mejor entrada vs primera)
  - Trend: ¿el profit esta mejorando o empeorando entre las ultimas N entradas?
  - Win rate trend
  - Cualquier insight relevante (ej. "drawdown bajo de 0.5% a 0.3%")

Ejemplo de respuesta:

> Entrada registrada en bitacora.md (commit abc1234).
>
> **Progreso evolutivo (12 entradas totales):**
> - Profit: $4,500 → $7,250 (+61% mejora desde primera entrada)
> - Win rate: 35% → 42% (+7pp)
> - Drawdown: 0.45% → 0.31% (mejora)
> - Profit factor: 1.05 → 1.45
>
> **Tendencia (ultimas 3 entradas):** profit subiendo consistentemente. Win rate estabilizado >40%. La direccion va bien.

## Formato del bitacora.md

Si no existe, crear con este header:

```markdown
# Bitacora de Evolucion de la Estrategia TLS

Historial de cambios y metricas del backtest. Cada fila = un commit con sus resultados.

**Convencion:**
- Profit USD/% positivo = mejora vs capital inicial
- DD % = drawdown maximo (menor es mejor)
- PF = profit factor (>1 = profitable, ideal >1.5)
- Win % = porcentaje de operaciones rentables

| Fecha | Commit | Cambio | Trades | Profit USD | Profit % | Win % | DD % | PF | Δ vs anterior |
|---|---|---|---|---|---|---|---|---|---|
```

## Ejemplos

### Ejemplo 1: Usuario dice "bitacora" sin imagen ni ship

User: "hagamos bitacora"

Claude (skill activated):
> Antes de registrar la entrada necesito 2 cosas:
> 1. ¿Ya hiciste el ship con Claude Code para que tenga el commit ID?
> 2. ¿Me puedes mandar el screenshot del informe de estrategia (Metricas)?

### Ejemplo 2: Usuario dice "bitacora" con imagen + confirmacion

User: "hagamos bitacora, ya hice ship" + screenshot adjunto

Claude (skill activated):
1. Run `git log -1` -> get commit hash + message
2. Read image -> extract metrics
3. Generate summary -> "Refactor multi-tier (...)"
4. Append row to bitacora.md
5. Show progress summary

### Ejemplo 3: Usuario quiere la primera entrada de la sesion

Si bitacora.md no existe, crearlo con el header. La primera entrada tendra "—" en la columna "Δ vs anterior".

## Edge cases

- **Imagen ilegible**: si los numeros no son claros, preguntar al usuario por los valores especificos
- **Multiple commits desde la ultima entrada**: tomar solo el ULTIMO commit; mencionar al usuario que hay mas commits sin registrar y preguntar si quiere registrar los anteriores tambien
- **Bitacora.md ya tiene una entrada con el mismo commit hash**: preguntar al usuario si quiere actualizar la entrada existente o crear una nueva (puede haber re-corrido el backtest con otro periodo)

## Skills relacionadas

- `strategy-debug`: para activar debug temporal antes de hacer ship
