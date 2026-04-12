# Strategy-777 — Framework de Estrategias en TradingView (Pine Script)

## Objetivo

Construir un framework modular en Pine Script que sirva como **herramienta de investigación y ejecución de estrategias de trading**. El enfoque es primero analítico: validar hipótesis, medir edge, e iterar. La ejecución operativa viene después.

## Filosofía Central

- No copiar estrategias de otros — **ingeniería inversa** para entender qué genera ventaja
- No solo automatizar — **investigar**: ¿cuándo entrar? ¿cuándo NO? ¿qué filtros ayudan?
- No rehacer todo en cada iteración — arquitectura **extensible desde el día uno**

## Lo que se construye

Un script tipo `strategy` en Pine Script que:

| Capacidad | Detalle |
|---|---|
| Señales | Long / Short / ambos (configurable) |
| Entradas | Tendencia, rompimientos, retests, momentum, volatilidad, estructura |
| Salidas | SL, TP, trailing stop, señal contraria, salida temporal/estructural |
| Riesgo | SL/TP configurables, ratio R/R, lógica dinámica opcional |
| Filtros | Sesiones, horarios, días, ventanas sin trading |
| Backtesting | 1m a diario, multi-timeframe, métricas completas |
| Visual | Cajas de riesgo/beneficio por operación, etiquetas de resultado, transparencia, modo oscuro |

## Flujo de Trabajo

```
Definición de reglas → Traducción a lógica → Backtest inicial
→ Ingeniería inversa → Refinamiento → Multi-timeframe
→ Evaluación de curva de equity → Iteración continua
```

## Métricas de Backtesting Requeridas

Win rate · Profit neto/bruto · Profit factor · Drawdown · Total de trades · Promedio por trade · Separación Long vs Short

## Visualización por Operación

Cada trade se representa como una **caja vertical dividida en dos zonas**:

- Verde (arriba): zona de objetivo con etiqueta de distancia, %, importe
- Roja (abajo): zona de stop con la misma información
- Etiqueta central: PyG, cantidad, ratio R/R — verde si ganó, roja si perdió

El diseño es semitransparente para no tapar las velas y funciona en fondos oscuros.

## Estructura del Código

```
inputs → cálculos → entradas → salidas → riesgo → visualización → debug
```

## Organización de Inputs

| Grupo | Parámetros |
|---|---|
| Trend | MA length, tipo de MA, confirmación HTF |
| Entry | Distancia de rompimiento, sensibilidad de retest, tamaño de vela |
| Exit | SL, TP, trailing |
| Risk | Ratio R/R, lógica dinámica |
| Session | Sesiones, horarios |
| Filters | Volatilidad (ATR), thresholds |
| Visual | Labels, zonas, transparencia |
| Debug | Activar/desactivar debug |

## Reglas de Desarrollo

1. Claude **no inventa reglas de trading** — el usuario define la lógica exacta
2. Código modular, legible y preparado para crecer
3. Cada parámetro relevante debe ser configurable desde los inputs
4. Los filtros deben poder activarse/desactivarse individualmente
5. La base nunca se rompe al agregar funcionalidades nuevas

## Documentación del Proyecto

Toda la especificación técnica se encuentra en [skills/](skills/):

| Archivo | Contenido |
|---|---|
| [00_project_overview.md](skills/00_project_overview.md) | Visión general y alcance |
| [01_strategy_goal.md](skills/01_strategy_goal.md) | Objetivo estratégico y filosófico |
| [02_pinescript_requirements.md](skills/02_pinescript_requirements.md) | Requisitos técnicos de Pine Script |
| [03_backtesting_requirements.md](skills/03_backtesting_requirements.md) | Métricas y configuración de backtesting |
| [04_parameters_and_inputs.md](skills/04_parameters_and_inputs.md) | Parámetros e inputs configurables |
| [05_analysis_workflow.md](skills/05_analysis_workflow.md) | Flujo de análisis por fases |
| [06_acceptance_criteria.md](skills/06_acceptance_criteria.md) | Criterios de aceptación |
| [07_claude_master_prompt.md](skills/07_claude_master_prompt.md) | Prompt maestro para Claude |
| [08_estilo_visual_entradas_salidas.md](skills/08_estilo_visual_entradas_salidas.md) | Guía de estilo visual de operaciones |

## Estado Actual

Repositorio recién iniciado — documentación de especificaciones completa, implementación Pine Script pendiente.
