# Objetivo de la Estrategia

## Objetivo General
Construir una estrategia en TradingView que ejecute decisiones de trading guiadas basadas en reglas definidas por el usuario.

## Propósito Estratégico
La estrategia debe tener eel siguiente motor:

1. Techo rojo para ventas
    - Un techo(caja) se debera dibujar cuando una vela roja sobrepase y cierre por debajo del extremo inferior del cuerpo de la vela verde anterior.
    - La caja se construye de la siguiente manera, el superior de la caja queda en linea con la mecha superior del par encontrado y su linea inferior va a ser el  extremo inferior del cuerpo de la vela anterior del par encontrado.
    - Un techo nuevo no se puede construir si al menos 1 vela del par de velas verde y rojo tocan una caja anterior sin haberla roto.
2. Piso verde para compras
    - Un piso(caja) se debera dibujar cuando una vela verde sobrepase y cierre por arriba del extremo superior del cuerpo de la vela roja anterior.
    - La caja se construye de la siguiente manera, el superior de la caja queda en linea con extremo superior del cuerpo de la vela anterior del par encontrado y su linea inferior va a ser la mecha mas inferior del par encontrado.
    - Un piso nuevo no se puede construir si al menos 1 vela del par de velas rojo y verde tocan una caja anterior sin haberla roto.
3. Cual es el largo de los techos o cajas rojas?
    - La caja va a ser dinamicas, va a ir extendiendo su ancho hacia la derecha por cada vela nueva encontrada.
    - la caja va a dejar de extenderse si una vela verde cierra con su cuerpo sobre la caja.
4. Cual es el largo de los pisos o cajas verdes?
    - Las caja va a ser dinamicas, va a ir extendiendo su ancho hacia la derecha por cada vela nueva encontrada.
    - la caja va a dejar de extenderse si una vela roja cierra con su cuerpo por debajo de la caja.
5. ¿Cuándo entrar al mercado?
    - Con una vela de continuacion indistintamente si esta dentro o fuera de la caja, si la vela que continue se encuentra despues de una caja roja, entonces entramos en ventas, si por el contrario es una caja verde, entramos en compras.
6. ¿Cuándo NO entrar?
    - Cuando hay muy pocos pips de rompimiento, esto debe ser configurable por el usuario.
    - Cuando le vela anterior es anterior a 100 pips.
7. ¿Cuándo salir?
    - debe ser definido  por el usuario, pero el stop loss va a ser de la cantidad de pips de la vela que rompio mas unos pocos pips mas definidos por el usuario, mientras que el profit el usuario debe definir si es un 1:1 o 1:2 segun este parametro de stop loss.
8. ¿Qué filtros mejoran la rentabilidad?

## Ingeniería Inversa
Debe permitir analizar estrategias inspiradas en otros traders, reconstruyendo:
- lógica de entrada,
- lógica de salida,
- dirección del mercado,
- filtros,
- confirmaciones,
- gestión de riesgo.

No se trata de copiar, sino de entender qué genera ventaja.

## Objetivo Operativo
Una vez validada, la estrategia debe:
- marcar oportunidades,
- evitar entradas inválidas,
- apoyar decisiones estructuradas.

## Flexibilidad
Debe permitir:
- agregar condiciones,
- activar/desactivar filtros,
- modificar parámetros,
- ajustar riesgo,
- probar variantes sin rehacer todo.
