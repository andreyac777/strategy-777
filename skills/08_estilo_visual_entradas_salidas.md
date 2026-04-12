# Guía de Estilo Visual para Entradas y Salidas

## Objetivo
Definir el estilo visual que debe usar la estrategia para marcar cada entrada y salida en TradingView, tomando como referencia el formato mostrado por el usuario: caja de operación con área de objetivo, área de stop, etiqueta central de resultado y niveles claramente visibles.

## Intención de Diseño
Cada trade debe ser fácil de leer en pocos segundos. La visualización no debe limitarse a marcar una flecha de entrada; debe representar la operación completa como un bloque visual informativo.

El diseño debe comunicar de inmediato:
- punto de entrada,
- objetivo,
- stop loss,
- relación riesgo/beneficio,
- resultado final del trade,
- y lectura visual de si la operación cerró en ganancia o pérdida.

## Elementos Visuales Obligatorios

### 1. Caja de Riesgo/Beneficio
Cada operación debe dibujarse como una caja vertical dividida en dos zonas:

- Zona superior en verde o turquesa para el objetivo.
- Zona inferior en rojo para el stop.

La caja debe iniciar en el precio de entrada y extenderse hasta el take profit y el stop loss.

### 2. Etiqueta Superior de Objetivo
Debe mostrarse una etiqueta en la parte superior de la caja con información similar a:
- distancia al objetivo,
- porcentaje,
- valor equivalente,
- importe.

Ejemplo de estructura:
- `Objetivo: 241 (0,336%) 241, Importe: 1.51`

### 3. Etiqueta Inferior de Stop
Debe mostrarse una etiqueta en la parte inferior con información similar a:
- distancia al stop,
- porcentaje,
- valor equivalente,
- importe.

Ejemplo de estructura:
- `Stop: 237 (0,331%) 237, Importe: 0.5`

### 4. Etiqueta Central de Resultado
Debe existir una etiqueta central y destacada que indique el resultado de la operación. Esta etiqueta debe cambiar según el cierre:

- Si la operación cierra positiva, la etiqueta central debe mostrarse en tono verde/turquesa.
- Si la operación cierra negativa, la etiqueta central debe mostrarse en tono rojo.

El texto debe incluir como mínimo:
- PyG cerrado,
- cantidad,
- ratio riesgo/beneficio.

Ejemplos de formato:
- `PyG Cerrado: 241, Cantidad: 0.002 ratio riesgo/beneficio: 1,02`
- `PyG Cerrado: -237, Cantidad: 0.002 ratio riesgo/beneficio: 1,02`

### 5. Niveles y Bordes
Los bordes del bloque deben verse limpios y definidos. La lectura ideal es:
- línea de entrada claramente distinguible,
- borde superior del take profit,
- borde inferior del stop,
- caja semitransparente para no tapar completamente las velas.

## Reglas de Color

### Operación Ganadora
- Área objetivo: verde/turquesa semitransparente.
- Etiqueta central: verde/turquesa.
- Texto: blanco o de alto contraste.

### Operación Perdedora
- Área stop: rojo semitransparente.
- Etiqueta central: roja.
- Texto: blanco o de alto contraste.

### Neutralidad del Gráfico
El diseño debe integrarse con fondos oscuros sin perder legibilidad.

## Reglas de Legibilidad
- Las etiquetas no deben montarse unas sobre otras en la medida de lo posible.
- El texto debe priorizar jerarquía visual: primero resultado, luego objetivo/stop.
- La transparencia debe permitir seguir viendo la acción del precio detrás.
- Debe evitarse saturar el gráfico cuando haya muchas operaciones cercanas.

## Comportamiento Esperado
La estrategia debe intentar representar cada operación con este estilo como primera opción visual.

Si TradingView o Pine Script tienen limitaciones técnicas, se debe construir la aproximación más cercana posible usando:
- `box.new`
- `line.new`
- `label.new`
- colores semitransparentes
- y actualización dinámica de objetos.

## Mejora Autónoma del Diseño
El sistema puede proponer mejoras visuales por iniciativa propia, pero debe respetar estas reglas:

1. Mantener la estructura base de caja de riesgo/beneficio.
2. No eliminar la etiqueta central de resultado.
3. No sacrificar claridad por estética.
4. Mejorar espaciado, alineación, contraste y jerarquía visual cuando detecte una alternativa superior.
5. Priorizar siempre lectura rápida, consistencia y limpieza.

## Principios de Mejora Permitidos
Se permite que Claude sugiera o implemente mejoras como:
- mejor alineación de labels,
- reducción de superposición,
- tamaños de texto adaptativos,
- cambio de transparencia para mejor lectura,
- bordes más limpios,
- mejor anclaje de etiquetas,
- y reorganización visual para distinguir entradas activas de cerradas.

## Restricciones de Mejora
No se debe:
- cambiar radicalmente la semántica del diseño,
- convertir la operación en una simple flecha,
- ocultar el bloque de riesgo/beneficio,
- o reducir la información visible hasta perder utilidad operativa.

## Requisito Técnico
La arquitectura visual debe ser lo bastante modular para permitir:
- activar o desactivar etiquetas,
- cambiar colores,
- cambiar transparencia,
- cambiar tamaño del texto,
- elegir si mostrar solo operaciones activas o también históricas,
- y escalar el diseño a mejoras futuras.

## Criterio de Éxito
Una visualización correcta debe permitir que el usuario identifique rápidamente:
- dónde entró,
- cuánto arriesgó,
- cuánto buscaba ganar,
- cómo cerró la operación,
- y cuál fue el resultado,

sin tener que abrir paneles externos ni leer información secundaria del backtester.
