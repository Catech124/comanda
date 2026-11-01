# Tonalli — comanda de sala y ticket de cocina

Aplicación web para tomar el pedido en sala y generar el ticket de comanda de cocina,
con el menú real de Tonalli (cocina de maíz y fuego).

**App:** https://catech124.github.io/comanda/

Funciona sin instalación ni servidor: un solo `index.html` (la tipografía y la foto del
plato van embebidas) y el estado vive en el navegador (`localStorage`).

---

## Qué resuelve

| Requisito de la práctica | Dónde está en la app |
|---|---|
| 20 mesas | Panel **Mesas** (01–20) con estado libre / comanda abierta / cuenta enviada |
| Mesa, comensales y mesero | Cabecera del papel: mesa seleccionada, contador de comensales, mesero a cargo |
| Menú por categorías operativas | Para comenzar 6 · Del comal 5 · Fuertes 6 · Postres 4 · Bebidas 7 (28 platillos) |
| Producto con tiempo y mods estándar | Vista **Catálogo**: nombre, descripción, tiempo estimado, precio y modificaciones estándar |
| Alta de platillo en un clic | Tocar la tarjeta del platillo agrega la partida |
| Notas especiales | Hoja de partida: término, modificaciones, alergia, comensal y nota libre |
| Ticket de comanda | Modal de ticket + impresión térmica 80 mm |
| Folio y hora exacta | `A-0001` + hora con segundos |
| Cantidad y nombre | Columna de cantidad + nombre del platillo |
| Modificaciones resaltadas | Recuadro en negritas dentro del ticket; alergias invertidas en negro |
| Prioridad / tiempo | Bloques por pase con minutos y hora estimada de salida |
| Salida a cocina | Vista **Cocina** con cronómetro contra el tiempo objetivo y "Marcar listo" |

## De dónde sale el menú

Los 28 platillos, precios, descripciones, la marca VEG y la "Experiencia Tonalli" de $320
se tomaron de `~/tonalli-menu/menu.html` (el menú de dos páginas que ya estaba diseñado).
La identidad visual de la app usa la misma paleta (verde profundo, oro, crema) y la
tipografía Bebas del menú original.

## Cómo se opera

1. **Sala**: se elige mesa (01–20), comensales y mesero.
2. Un clic en el platillo agrega la partida. Otro clic en la misma tarjeta suma cantidad.
3. Tocar una partida abre la hoja de modificaciones: término, modificaciones estándar,
   alergia declarada, comensal al que se sirve, nota libre y prioridad alta.
4. **Enviar a cocina** genera el folio, sella la hora y abre el ticket listo para imprimir.
5. **Cocina** muestra las comandas en orden de llegada con el cronómetro contra el tiempo
   objetivo. Al marcar una comanda como lista, el cronómetro **se detiene** y queda el
   tiempo de preparación real; el cronómetro solo corre mientras la comanda está en cocina.
6. Una mesa puede recibir varias comandas (rondas). **Cerrar mesa** libera la mesa.

El tiempo estimado se calcula por pases: se suman los máximos de 1er tiempo (entradas y
comal salen juntos), fuerte y postre; la barra no suma porque sale de inmediato.

## Guion de demostración (3 tiempos)

1. Elegir la **mesa 7**, subir a 3 comensales y dejar el mesero.
2. **Para comenzar** → *Aguachile verde*.
3. **Del comal** → *Tlayuda Tonalli*.
4. **Fuertes** → *Pulpo a las brasas*. Abrir la partida: **Término medio**, **Extra limón**,
   **alergia Mariscos** y la nota `Sin sal, limón aparte`.
5. **Postres** → *Tamal de chocolate*.
6. **Enviar a cocina** → ticket con folio, hora, mesa, cantidades, modificaciones en
   recuadro y los pases con su tiempo (barra → 1er tiempo → comal → fuerte → postre).
7. **Ver en cocina** → la comanda aparece con su cronómetro; **Marcar listo** lo detiene.
8. Opcional: **Imprimir 80 mm** para el pase de cocina en papel térmico.

## Edición

- Menú, meseros, términos y alérgenos están al inicio del `<script>` de `index.html`
  (`MENU`, `MESEROS`, `TERMINOS`, `ALERGIAS`).
- Cada platillo: `{id, n (nombre), c (categoría), p (precio), m (minutos), d (descripción),
  op (modificaciones), term, veg}`.
- Estado en `localStorage` con la clave `comanda.tonalli.v1`. Para reiniciar la demo:
  consola del navegador → `localStorage.clear()` y recargar.

## Detalles técnicos

- Un solo archivo `index.html`, sin dependencias ni CDN; la fuente Bebas y la foto del
  mole negro van embebidas en base64, así que carga y funciona sin red.
- Impresión: hoja `@page` de 80 mm × 250 mm; solo se imprime el ticket.
- Accesible: controles con `aria-pressed` / `aria-selected`, foco visible, contraste AA y
  navegación por teclado en la comanda.
- Responsive: tres columnas en escritorio, mesas en tira horizontal y comanda a ancho
  completo en móvil.
