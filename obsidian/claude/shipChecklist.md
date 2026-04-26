---
tags: [claude, ship, checklist, entrega]
updated: 2026-04-26
---

# Ship Checklist — Validación y Commit

> [!info] Activación
> Ejecutar `/ship` para correr este proceso automáticamente.

---

## 1. Revisar Estado del Repositorio

```bash
git status
git diff
```

Identificar el archivo `.pine` principal.

---

## 2. Validar Criterios de Aceptación

> [!todo] Checklist funcional
> - [ ] Es tipo `strategy`
> - [ ] Genera entradas/salidas (long/short)
> - [ ] Soporta backtesting con métricas visibles
> - [ ] Funciona en múltiples temporalidades
> - [ ] Parámetros agrupados por categoría
> - [ ] Filtros activables/desactivables
> - [ ] Visualización de caja R/R por operación
> - [ ] Código modular y legible

→ Detalle completo en [[../reference/acceptanceCriteria]]

---

## 3. Validar Reglas Pine Script

> [!todo] Checklist de sintaxis
> - [ ] `//@version=5` es la línea 1 absoluta
> - [ ] `strategy()` tiene primer param en misma línea
> - [ ] Cero non-ASCII: `LC_ALL=C grep -n '[^ -~]' archivo.pine`
> - [ ] Cero `(` solos al final de línea: `grep -n "($" archivo.pine`
> - [ ] Cero `//` dentro de expresiones multi-línea

→ Detalle completo en [[../reference/pineScriptSyntax]]

---

## 4. Reporte de Entrega

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
- <issues si los hay, o "ninguno">
```

---

## 5. Commit

> [!warning] Solo si ESTADO = LISTO
> ```bash
> git add <archivos .pine y skills/ relevantes>
> git commit -m "feat(strategy): <descripcion corta>
>
> Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"
> ```
> **NO hacer push** a menos que el usuario lo pida explícitamente.
> Nunca agregar `.env`, archivos temporales ni binarios.
