---
tags: [referencia, workflow, fases]
updated: 2026-04-26
---

# Flujo de Análisis

Workflow estándar para desarrollar y validar una estrategia en este framework.

---

```
Definición → Traducción → Backtest inicial
→ Ingeniería Inversa → Refinamiento → Multi-TF
→ Evaluación de equity → Iteración
```

---

## Fases

| Fase | Qué hacer |
|------|-----------|
| **1. Definición** | El usuario define: entradas, salidas, filtros, riesgo |
| **2. Traducción** | Convertir reglas de trading a lógica Pine Script exacta |
| **3. Backtest Inicial** | Evaluar frecuencia, win rate, drawdown básico |
| **4. Ingeniería Inversa** | Analizar qué genera ventaja: tendencia, timing, volatilidad, filtros |
| **5. Refinamiento** | Mejorar precisión, consistencia, calidad de trades |
| **6. Multi-Timeframe** | Validar resultados en distintas temporalidades |
| **7. Evaluación** | Analizar curva de equity, estabilidad, sensibilidad de parámetros |
| **8. Iteración** | Agregar filtros, modos, mejoras sin romper la base |

---

## Preguntas Clave en Cada Fase

- **¿Cuándo entrar?** → [[../strategy/entryRules]]
- **¿Cuándo NO entrar?** → [[../strategy/entryRules#When NOT to Enter]]
- **¿Qué filtros mejoran el win rate?** → Backtesting comparativo con/sin filtro
- **¿El edge es real o es overfitting?** → Validar en períodos distintos a los usados para optimizar
