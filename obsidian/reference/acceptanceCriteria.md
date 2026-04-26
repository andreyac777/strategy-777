---
tags: [referencia, checklist, entrega]
updated: 2026-04-26
---

# Criterios de Aceptación

Checklist que debe cumplir cualquier script antes de hacer commit.

→ Ver el proceso completo en [[../claude/shipChecklist]]

---

## Funcionales

- [ ] Script tipo `strategy` (no `indicator`)
- [ ] Genera entradas y salidas (long/short)
- [ ] Permite backtesting con métricas visibles
- [ ] Funciona en múltiples temporalidades
- [ ] Soporta múltiples estrategias (módulos)

## Métricas

- [ ] Win rate visible
- [ ] Profit visible
- [ ] Drawdown visible

## Flexibilidad

- [ ] Parámetros configurables desde inputs
- [ ] Filtros activables/desactivables individualmente

## Usabilidad

- [ ] Código claro y comentado
- [ ] Inputs organizados por grupo
- [ ] Visualización de caja R/R por operación

## Calidad

- [ ] Código modular
- [ ] Preparado para iteraciones
- [ ] Sin caracteres non-ASCII
- [ ] Sin errores de sintaxis Pine Script

→ Ver reglas de sintaxis en [[pineScriptSyntax]]
