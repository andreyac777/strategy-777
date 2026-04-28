---
tags:
  - indicador
  - strategy
  - pine-script
  - ict
archivo: rich-ko.pine
updated: 2026-04-26
cssclasses:
---

# Rich KO

Estrategia ICT (Inner Circle Trader) que estudia el flujo institucional en las sesiones London (Context) y Nueva York (NY). El objetivo es identificar zonas de liquidez, estructuras de mercado y entradas de alta probabilidad alineadas con el sesgo institucional del dia.

---

## Logica Implementada

### Sesiones
- **Context (London):** Se detecta por hora local configurable. Genera el rango de referencia (LH/LL).
- **NY Session:** Sesion de ejecucion. Las entradas ocurren aqui siguiendo el sesgo formado en Context.
- Las franjas horarias se dibujan como bandas verticales fijas (violet = Context, cyan = NY).

### LH / LL (Lower High / Lower Low)
- Se registran el high y low maximos dentro de Context.
- Al cerrar Context, se trazan lineas blancas solidas desde la vela que origino el extremo.
- La linea se extiende barra a barra hasta que el precio la toca o cierra NY.
- Al romperse por precio: se emite label **"LH Break"** (encima del precio) o **"LL Break"** (debajo).

### FVG (Fair Value Gap)
- Patron de 3 velas: gap entre high[2] y low del bar actual (bull) o gap entre low[2] y high (bear).
- Solo se detecta dentro de Context.
- Se dibuja como caja con linea de equilibrio (50%) al centro.
- Expira si el precio cierra dentro del gap o al cierre de NY (la caja permanece en el historial).

### OB (Order Block)
- Vela opuesta inmediatamente antes de un movimiento de desplazamiento (breakout del high/low anterior).
- Solo se detecta dentro de Context.
- Expira igual que FVG.

### Volume Profile (FRVP)
- Se acumula el volumen barra a barra durante Context.
- Al cierre de Context se distribuye en niveles entre LH y LL.
- Se dibuja dentro de la franja Context (lado izquierdo).
- El nivel con mas volumen (POC) se marca en amarillo.

---

## Conceptos Pendientes de Implementar

> [!todo] Roadmap ICT Rich KO
> Los siguientes conceptos del framework ICT fueron identificados en el transcript del autor
> pero aun no estan codificados en rich-ko.pine.

### 1. Equilibrium (50% del rango Context)
- Punto medio exacto entre LH y LL: `eq = (lhLevel + llLevel) / 2`
- Linea horizontal discontinua en el chart durante NY.
- Es el nivel de referencia para clasificar precio como **Discount** (por debajo) o **Premium** (por encima).
- Entradas long se buscan en Discount; entradas short en Premium.

### 2. Clasificacion del perfil de sesion
- Context **bajista:** LH + LL formados (estructura de caida). Sesgo del dia = short.
- Context **alcista:** HH + HL formados. Sesgo del dia = long.
- Actualmente la estrategia registra LH/LL pero no clasifica el sesgo ni lo muestra como bias.

### 3. Zonas Discount / Premium
- **Discount:** precio < Equilibrium → zona de compra institucional.
- **Premium:** precio > Equilibrium → zona de venta institucional.
- Visualizacion sugerida: sombrear la mitad inferior del rango Context en verde, la superior en rojo.

### 4. Judas Swing
- Movimiento falso al inicio de NY (primeros 15-30 min) que va en direccion **contraria** al sesgo.
- Objetivo: recoger la liquidez de los stops del lado equivocado.
- Señal: precio sube brevemente por encima de LH (liquidity sweep) antes de revertir hacia abajo (si sesgo = short), o baja por debajo de LL antes de subir (si sesgo = long).
- Implementacion sugerida: detectar el primer toque/break de LH o LL dentro de los primeros 30 min de NY y marcar como posible Judas.

### 5. MSS (Market Structure Shift)
- Primera señal de que el Judas termino y el precio revierte hacia el sesgo real.
- En estructura bajista: primer Higher High (HH) roto seguido de un Lower Low mas bajo → MSS confirmado.
- En estructura alcista: primer Lower Low (LL) roto seguido de un Higher High → MSS confirmado.
- El MSS es la confirmacion de entrada; no se entra antes de que ocurra.

### 6. Logica de entrada post-MSS
- Despues del MSS, el precio suele retroceder a un **FVG** o **OB** en la zona de Discount/Premium.
- La entrada va en la direccion del sesgo (confirmado por MSS), con SL por debajo del low del Judas.
- El TP primario es el extremo opuesto del rango Context (LH o LL segun la direccion).

### 7. Macro de las 10 AM (NY local)
- Ventana: 9:50 AM - 10:10 AM hora NY.
- Alta probabilidad de reversal o aceleracion de la tendencia del dia.
- ICT la usa para confirmar el sesgo o como segundo punto de entrada.
- Implementacion sugerida: marcar esta ventana como banda temporal opcional (similar a las bandas de sesion).

### 8. Estado "intacto" vs "roto" de LH/LL
- Actualmente la linea muere cuando el precio la toca y se emite el label Break.
- ICT distingue entre una linea **tocada** (precio rozo pero no cerro por encima/debajo) y una linea **rota** (cierre confirmado).
- Implementacion sugerida: separar la condicion de expiry entre toque de mecha (`high >= lhLevel`) y cierre confirmado (`close >= lhLevel`), y usar colores distintos para cada estado.

---

## Parametros

| Input | Descripcion | Default |
|-------|-------------|---------|
| `i_pip` | Tamano del pip | 1.0 |
| `i_utcOffset` | Desfase UTC (Colombia = -6) | -6 |
| `i_ctxStart` | Hora local inicio Context | 2 |
| `i_nyStart` | Hora local inicio NY / cierre Context | 8 |
| `i_nyEndHour` | Hora local cierre NY | 16 |
| `i_nyEndMin` | Minuto cierre NY | 0 |
| `i_showLabels` | Mostrar etiquetas de sesion | true |
| `i_showVP` | Mostrar Volume Profile | true |
| `i_vpLevels` | Niveles del VP | 15 |
| `i_vpWidth` | Ancho max del VP (barras) | 15 |
| `i_fvgMin` | Tamano minimo de FVG (pips) | 3.0 |
