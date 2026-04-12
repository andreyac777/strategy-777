Ejecuta los siguientes pasos en orden para validar y entregar la estrategia Pine Script.

## 1. Revisar estado del repositorio

- Ejecuta `git status` para ver que archivos han cambiado.
- Ejecuta `git diff` para revisar los cambios pendientes.
- Identifica el archivo `.pine` principal de la estrategia.

## 2. Validar criterios de aceptacion

Lee `skills/06_acceptance_criteria.md` y verifica que el script cumpla:

- [ ] Es tipo `strategy`, no `indicator`
- [ ] Genera entradas y salidas (long/short)
- [ ] Soporta backtesting con metricas visibles
- [ ] Funciona en multiples temporalidades
- [ ] Parametros agrupados por categoria (Trend, Entry, Risk, Session, Visual, Debug)
- [ ] Filtros activables/desactivables individualmente
- [ ] Visualizacion de caja riesgo/beneficio por operacion
- [ ] Codigo modular y legible

## 3. Validar reglas Pine Script

Lee `skills/10_pinescript_syntax_rules.md` y verifica:

- [ ] //@version=5 es la linea 1 absoluta del archivo
- [ ] strategy() tiene el primer parametro en la misma linea que el parentesis
- [ ] Cero caracteres non-ASCII (ejecuta: `LC_ALL=C grep -n '[^ -~]' archivo.pine`)
- [ ] Cero parentesis solos al final de linea (ejecuta: `grep -n "($" archivo.pine`)
- [ ] Cero comentarios // dentro de expresiones multi-linea

## 4. Reportar resultado

Genera un resumen con este formato exacto:

```
SHIP REPORT - Strategy-777
===========================
Archivo principal: <nombre>.pine

CRITERIOS DE ACEPTACION:
  [x/o] Es tipo strategy
  [x/o] Genera senales long/short
  [x/o] Backtesting habilitado
  [x/o] Inputs organizados por grupo
  [x/o] Visual R/R implementado
  [x/o] Filtros activables

PINE SCRIPT SYNTAX:
  [x/o] //@version=5 en linea 1
  [x/o] strategy() con primer param en misma linea
  [x/o] Cero non-ASCII
  [x/o] Cero ( al final de linea

ESTADO: LISTO / PENDIENTE

Observaciones:
- <lista de issues si los hay, o "ninguno">
```

## 5. Commit (solo si ESTADO es LISTO)

Si todo esta en orden:

- Agrega los archivos `.pine` y `skills/` relevantes al staging
- Nunca agregar `.env`, archivos temporales ni binarios
- Crea el commit con el formato:

```
feat(strategy): <descripcion corta de lo que se añadio o cambio>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```

- Muestra el hash del commit al usuario

## Notas

- NO hacer push a remote a menos que el usuario lo pida explicitamente.
- Si hay criterios no cumplidos, listarlos y NO hacer commit.
- Si $ARGUMENTS contiene un mensaje, usarlo como descripcion del commit.
