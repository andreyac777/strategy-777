---
tags: [estrategia, visual, diseño]
updated: 2026-04-26
---

# Estilo Visual de Entradas y Salidas

Cada trade se representa como una **caja vertical dividida en dos zonas** que muestra la operación completa de un vistazo.

---

## Estructura de la Caja

```
┌─────────────────────────────────┐
│  Objetivo: 241 (0.336%) $1.51   │  ← etiqueta superior verde
├─────────────────────────────────┤
│                                 │
│  Zona verde (take profit)       │
│                                 │
├────── ENTRADA ──────────────────┤  ← precio de entrada
│                                 │
│  Zona roja (stop loss)          │
│                                 │
├─────────────────────────────────┤
│  Stop: 237 (0.331%) $0.50       │  ← etiqueta inferior roja
└─────────────────────────────────┘

    PyG Cerrado: 241 | 0.002 | RR: 1.02   ← etiqueta central de resultado
```

---

## Elementos Obligatorios

| Elemento | Descripción |
|----------|-------------|
| Zona superior | Verde/turquesa semitransparente — área de take profit |
| Zona inferior | Roja semitransparente — área de stop loss |
| Etiqueta superior | Distancia · % · importe del objetivo |
| Etiqueta inferior | Distancia · % · importe del stop |
| Etiqueta central | PyG · Cantidad · Ratio R/R — color según resultado |

---

## Reglas de Color

| Estado | Zona objetivo | Etiqueta central |
|--------|--------------|-----------------|
| Ganadora | Verde/turquesa | Verde |
| Perdedora | Rojo (predomina stop) | Rojo |
| Activa | Semitransparente | — |

---

## Implementación en Pine Script

```pine
box.new(...)   // zona TP y SL
line.new(...)  // nivel de entrada
label.new(...) // etiquetas superior, inferior y central
```

- Semitransparente para no tapar las velas
- Diseño para fondos oscuros

---

## Reglas de Legibilidad

- Etiquetas no deben montarse unas sobre otras
- Jerarquía visual: resultado > objetivo/stop
- Transparencia permite seguir viendo la acción del precio
- No saturar el gráfico con muchas operaciones cercanas

---

## Módulos Configurables

- Activar/desactivar etiquetas individualmente
- Cambiar colores y transparencia
- Mostrar solo operaciones activas o también históricas
- Cambiar tamaño del texto
