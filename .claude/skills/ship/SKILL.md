---
name: ship
description: Valida y prepara la estrategia Pine Script para su uso en TradingView. Úsalo cuando el script esté listo para ser probado o publicado.
disable-model-invocation: true
allowed-tools: Bash(git *) Read Grep Glob
---

# Ship — Validar y entregar la estrategia

Ejecuta los siguientes pasos en orden para preparar la estrategia para TradingView.

## 1. Revisar estado del repositorio

- Ejecuta `git status` para ver qué archivos han cambiado.
- Ejecuta `git diff` para revisar los cambios pendientes.
- Identifica el archivo `.pine` principal de la estrategia.

## 2. Validar criterios de aceptación

Lee [skills/06_acceptance_criteria.md](../../skills/06_acceptance_criteria.md) y verifica que el script cumpla:

- [ ] Es tipo `strategy`, no `indicator`
- [ ] Genera entradas y salidas (long/short)
- [ ] Soporta backtesting con métricas visibles
- [ ] Funciona en múltiples temporalidades
- [ ] Parámetros agrupados por categoría (Trend, Entry, Exit, Risk, Session, Filters, Visual, Debug)
- [ ] Filtros activables/desactivables individualmente
- [ ] Visualización de caja riesgo/beneficio por operación
- [ ] Código modular y legible

## 3. Revisar calidad del código Pine Script

Lee el archivo `.pine` principal y verifica:

- Que todos los grupos de inputs estén presentes y organizados
- Que las secciones estén separadas con comentarios: `// === INPUTS ===`, `// === CALCULATIONS ===`, `// === ENTRIES ===`, `// === EXITS ===`, `// === RISK ===`, `// === VISUALIZATION ===`, `// === DEBUG ===`
- Que no haya variables sin usar o lógica muerta
- Que los colores del visual sean semitransparentes (compatibles con fondo oscuro)

## 4. Reportar resultado

Genera un resumen con:

```
SHIP REPORT — Strategy-777
===========================
Archivo principal: <nombre>.pine
Versión Pine Script: v5 / v6

CRITERIOS DE ACEPTACION:
  [x/o] Es tipo strategy
  [x/o] Genera señales long/short
  [x/o] Backtesting habilitado
  [x/o] Inputs organizados por grupo
  [x/o] Visual R/R implementado
  [x/o] Filtros activables

ESTADO: LISTO / PENDIENTE

Observaciones:
- <lista de issues si los hay>
```

## 5. Commit (solo si el estado es LISTO)

Si todo está en orden:

- Agrega los archivos relevantes al staging (nunca `.env` ni archivos temporales)
- Crea el commit con el formato:
  ```
  feat(strategy): <descripción corta de lo que se añadió o cambió>

  Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
  ```
- Muestra el hash del commit al usuario

## Notas

- No hacer push a remote a menos que el usuario lo indique explícitamente.
- Si hay criterios no cumplidos, listarlos claramente y **no hacer commit**.
- Si $ARGUMENTS contiene un mensaje, usarlo como descripción del commit.
