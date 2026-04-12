# Requisitos de Pine Script

## Objetivo Técnico
Implementar la estrategia en Pine Script (última versión estable).

## Tipo de Script
Debe ser un `strategy`, no solo `indicator`, para:
- simular trades,
- calcular métricas,
- permitir backtesting.

## Requisitos Funcionales

### 1. Dirección
- Longs
- Shorts
- Opción de activar solo uno

### 2. Entradas
Debe soportar lógica configurable:
- tendencia,
- medias móviles,
- rompimientos,
- retests,
- momentum,
- volatilidad,
- estructura del precio.

### 3. Salidas
Debe soportar:
- stop loss,
- take profit,
- trailing stop,
- cierre por señal contraria,
- salida por tiempo,
- salida estructural.

### 4. Gestión de Riesgo
- stop configurable,
- take profit configurable,
- ratio riesgo/beneficio,
- lógica dinámica opcional.

### 5. Filtros de Tiempo
- sesiones,
- días,
- horarios,
- ventanas sin trading.

### 6. Visualización
- entradas/salidas,
- zonas SL/TP,
- etiquetas,
- zonas de bias,
- debug opcional.

## Calidad del Código
- modular,
- legible,
- extensible,
- bien organizado.

## Diseño
Debe permitir crecimiento continuo sin romper la base.
