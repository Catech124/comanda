# Comanda — toma de pedidos y ticket de cocina

Aplicación web para tomar el pedido en sala y generar el ticket de comanda de cocina.
Funciona sin instalación, sin servidor y sin conexión después de la primera carga: todo
el estado vive en el navegador (`localStorage`).

**App:** https://catech124.github.io/comanda/

---

## Qué resuelve

| Requisito de la práctica | Dónde está en la app |
|---|---|
| 20 mesas | Panel **Mesas** (01–20) con estado libre / comanda abierta / cuenta enviada |
| Mesa, comensales y mesero | Cabecera del papel de comanda: mesa seleccionada, contador de comensales, mesero a cargo |
| Menú por categorías operativas | Entradas, Fuertes, Postres, Bebidas, Guarniciones (25 productos) |
| Producto con tiempo y mods estándar | Vista **Catálogo**: nombre, tiempo estimado, precio y modificaciones estándar |
| Alta de platillo en un clic | Tocar la tarjeta del platillo agrega la partida |
| Notas especiales | Editor de partida: término, modificaciones, alergia, comensal y nota libre |
| Ticket de comanda | Modal de ticket + impresión térmica 80 mm |
| Folio y hora exacta | `A-0001`, hora con segundos |
| Cantidad y nombre | Columna de cantidad + nombre del platillo |
| Modificaciones resaltadas | Recuadro en negritas dentro del ticket; alergias invertidas en negro |
| Prioridad / tiempo | Bloques por pase: `BARRA · SERVIR YA`, `1er TIEMPO · ENTRADA`, `2º TIEMPO · FUERTE`, `3er TIEMPO · POSTRE`, `ACOMPAÑAMIENTO` con minutos por bloque y hora estimada de salida |

## Cómo se opera

1. **Sala**: se elige mesa (01–20), comensales y mesero.
2. Un clic en el platillo agrega la partida. Otro clic en la misma tarjeta suma cantidad.
3. Tocar una partida abre el editor: término de la carne, modificaciones, alergia declarada,
   comensal al que se sirve, nota libre y prioridad alta.
4. **Enviar a cocina** genera folio, sella la hora y abre el ticket listo para imprimir.
5. **Cocina** muestra las comandas en orden de llegada con cronómetro contra el tiempo
   objetivo; cada comanda se marca como lista.
6. Una mesa puede recibir varias comandas (rondas). **Cerrar mesa** libera la mesa.

El tiempo estimado de la comanda se calcula por pases: se suman los tiempos máximos de
entrada, fuerte y postre (la cocina sirve en secuencia); barra y guarniciones no suman
porque salen en paralelo.

## Guion de demostración en vivo (3 tiempos)

1. Abrir la app y elegir la **mesa 7**, subir comensales a 3 y dejar el mesero.
2. **Entradas** → *Queso fundido con chorizo* (un clic).
3. **Fuertes** → *Rib eye 400 g*. Tocar la partida y marcar **3/4**, **Salsa aparte**,
   **alergia Mariscos** y la nota `Sin ajonjolí`. Cerrar el editor.
4. **Postres** → *Flan de cajeta*.
5. **Enviar a cocina** → se proyecta el ticket con folio, hora, mesa, cantidades,
   modificaciones en recuadro y pases con su tiempo.
6. **Ver en cocina** → la comanda aparece con su cronómetro; **Marcar listo**.
7. Opcional: **Imprimir 80 mm** para el pase de cocina en papel térmico.

## Datos y edición del menú

- El menú está en el arreglo `MENU` al inicio del `<script>` de `index.html`:
  `{id, n (nombre), c (categoría), p (precio), m (minutos), op (modificaciones), term}`.
- Meseros en `MESEROS`. Alérgenos en `ALERGIAS`. Términos en `TERMINOS`.
- El estado (comandas, folios, mesas) se guarda en `localStorage` con la clave `comanda.v2`.
  Para reiniciar la demo: consola del navegador → `localStorage.clear()` y recargar.

## Detalles técnicos

- Un solo archivo `index.html` (HTML + CSS + JS), sin dependencias ni CDN: carga en
  cualquier navegador moderno y funciona sin red.
- Impresión: hoja `@page` de 80 mm; el ticket es lo único que se imprime.
- Accesible: navegación por teclado en la comanda, `aria-pressed`/`aria-selected` en
  controles de estado, foco visible y contraste AA.
- Responsive: tres columnas en escritorio, panel de mesas en tira horizontal y ticket a
  ancho completo en móvil.
