# Café Altiplano · Proyecto final

Sitio adaptable de cuatro páginas, hecho solo con HTML y CSS (sin JavaScript, sin Flexbox, sin frameworks).

## 1. Mapa del sitio

| Página | Qué hay |
| --- | --- |
| `index.html` | Portada, historia del café, horario, galería de seis favoritos, proceso de preparación, una opinión de cliente y enlaces a las sedes. |
| `carta.html` | Índice con anclas y cuatro tablas de precios (bebidas calientes, bebidas frías, panadería y café en grano), notas, alérgenos y cómo pedir el grano. |
| `sedes.html` | Tres sedes con fotografía, dirección, teléfono, horario y servicios, más consejos antes de venir. |
| `reservas.html` | Formulario de reserva (datos, visita y preferencias) y dos cajas de ayuda. |

## 2. Tabla de medidas

Columnas del contenedor principal (`.pagina`). Probado en Chromium; el desplazamiento horizontal se midió con `scrollWidth` frente a `clientWidth`.

| Página | 500 px | 800 px | 1200 px |
| --- | --- | --- | --- |
| index | 1 columna, sin scroll | 2 columnas, sin scroll | 3 columnas, sin scroll |
| carta | 1 columna, sin scroll | 2 columnas, sin scroll | 3 columnas, sin scroll |
| sedes | 1 columna, sin scroll | 2 columnas, sin scroll | 3 columnas, sin scroll |
| reservas | 1 columna, sin scroll | 2 columnas, sin scroll | 3 columnas, sin scroll |

En las tablas de precios, a 500 px solo se desplaza el contenedor de la tabla, no la página.

## 3. Justificación de cinco unidades (RF-06.5)

1. **`rem` en los tamaños de letra.** Con `html { font-size: 100% }`, todo escala con la preferencia de tamaño del navegador de cada persona. Con `px` no lo respetaría.
2. **`rem` en espacios y anchos máximos** (`gap`, `padding`, `max-width: 75rem`). El ritmo de espacios crece o se reduce junto con la letra, así que las proporciones no se rompen.
3. **`ch` en los párrafos** (`max-width: 65ch`). El ancho se mide en caracteres, que es lo que importa para la lectura, y se mantiene cómodo aunque cambie la fuente.
4. **`vh` en la portada** (`min-height: 60vh` en el inicio y `28vh` en las demás). La portada guarda una proporción de la pantalla y el contenido nunca la desborda porque es una altura *mínima*.
5. **`%` y `fr` en el ancho de tablas y columnas** (`width: 100%` en la tabla y `minmax(0, 1fr)` en la cuadrícula). Se adaptan al contenedor sin conocer el ancho de la pantalla. Solo uso `px` en bordes finos y en las consultas de medios.

## 4. Decisiones

**Móvil primero.** La hoja base describe el teléfono (una columna) y `layout.css` agrega capas con `@media (min-width: …)`. Así el teléfono, que es donde más se mira el sitio, no carga reglas que deba deshacer.

**Puntos de ruptura de 600 y 900 px.** A 600 px el contenido ya aguanta dos columnas legibles; a 900 px caben tres sin apretar las tarjetas con tablas. Las consultas están escritas de menor a mayor.

**Subgrid.** `.pagina` define las columnas y `main` las hereda con `subgrid`. Así la cabecera, la portada y el pie ocupan todo el ancho (`grid-column: 1 / -1`) y las tarjetas del contenido usan las mismas columnas. La galería, en cambio, se acomoda sola con `repeat(auto-fit, minmax(…, 1fr))`.

## 5. Dificultades

- **Colores con transparencia sin "colores sueltos".** `rgba()` normalmente lleva números literales, pero la paleta debía vivir solo en `:root`. Lo resolví guardando los canales (`--rgb-pizarra: 28 43 51`) y usando `rgb(var(--rgb-pizarra) / 0.5)` donde hace falta transparencia.
- **Una tabla ancha estiraba la columna y causaba scroll horizontal.** Un elemento de la cuadrícula no se encoge por debajo de su contenido. Lo resolví con `min-width: 0` en los hijos de `main` y `overflow-x: auto` en el contenedor de cada tabla.
- **Una columna sobrante al pasar de 3 a 2 columnas.** Con `grid-auto-flow: dense`, las tarjetas pequeñas rellenan los huecos que dejan las anchas.
