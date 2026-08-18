# theme/

Código de tema versionado acá para no perderlo dentro de Shopify.

Shopify no versiona los archivos de tema: si alguien edita o borra un snippet desde el
admin, no hay historial ni forma de recuperarlo. Lo que viva en esta carpeta es la
copia de referencia.

## `snippets/albatro-sticky-atc.liquid`

Barra fija de "Agregar al carrito" para mobile. El tema **Horizon** trae
`sticky_details_desktop` pero no tiene equivalente para mobile, así que hay que
agregarlo.

**Instalado en:** tema `MOBILE FIX — sticky ATC + swatches (PUBLICAR ESTA)` (ID
`212411449597`), sin publicar. Se renderiza desde `layout/theme.liquid`, al final del
`<body>`:

```liquid
{% render 'albatro-effects' %}
{% comment %} Barra fija de "Agregar al carrito" en mobile (solo fichas de producto) {% endcomment %}
{% render 'albatro-sticky-atc' %}
```

### Cómo funciona

No duplica la lógica del carrito. El botón de la barra hace `click()` sobre el botón
real de la ficha, así que la selección de variante, el AJAX y el cart drawer del tema
se reutilizan tal cual.

- Aparece solo cuando el botón real **ya salió de pantalla** (`IntersectionObserver`).
- Espeja el estado real del botón — etiqueta y `disabled` — y se re-sincroniza cuando
  el tema re-renderiza el formulario al cambiar de variante.
- Se esconde si hay un `dialog[open]`, para no tapar el cart drawer.
- Se mueve al `<body>` al iniciar, para que `position: fixed` se ancle al viewport y no
  a un ancestro con `transform`.
- Solo bajo 750 px. Área táctil de 48 px. Respeta `safe-area-inset-bottom`.
- Si no encuentra el botón real, **no renderiza nada**. Falla en silencio.

### Por qué la clase se llama `.albatro-sticky-atc`

Hay dos reglas en el tema apagando barras sticky de intentos anteriores:

```css
.ag-atc-bar { display: none !important; }          /* snippets/albatro-effects.liquid */
.sticky-add-to-cart { display: none !important; }  /* snippets/albatro-init.liquid */
```

El nombre nuevo no colisiona con ninguna. **No renombrar la clase a `ag-atc-bar` ni a
`sticky-add-to-cart`** — quedaría oculta.

### Si hay que revertir

Borrar el tema duplicado, o quitar la línea `{% render 'albatro-sticky-atc' %}` de
`layout/theme.liquid`. El tema publicado no fue modificado.

## `snippets/albatro-popup-golf.liquid`

Popup "El Putt Albatro" — el minijuego de captura de email que da `FUNDADOR15`. Este
archivo reemplaza al que ya vivía en el tema, no es nuevo.

**Instalado en:** el mismo tema preview `212411449597`, junto al sticky-ATC. Un solo
"Publicar" activa los dos cambios.

### Qué cambió y qué no

Se comparó primero contra dos prototipos aislados (2D con arte mejorado vs. 3D real con
Three.js/WebGL) para decidir con datos antes de tocar el archivo en vivo — el dueño
eligió el 2D: mismo motor, cero riesgo nuevo de rendimiento en mobile, ~620 KB más
liviano que la alternativa en WebGL.

El parche se aplicó con reemplazo de texto verificado — cada bloque se confirmó que
aparecía **exactamente una vez** antes de reemplazarlo, y se diffearon después las 19
funciones que no debían tocarse (`power()`, `physics()`, `sink()`, `miss()`,
`pointerdown/move/up`, el formulario de Shopify, el manejo de `localStorage`) contra el
original — todas salieron **byte-idénticas**. Solo cambió el dibujo:

- `drawSky`, `drawGreen`, `drawHole`, `drawFlag`, `drawGolfer`, `drawBall` — mismo
  código parametrizado en `GX/GY/GW/GH/hole/start` que ya traía el original, solo con
  mejor luz, sombra en dos capas y proporciones del golfista.
- `drawGrassFlecks` (nueva) — chispas de pasto al golpear la pelota.
- Parallax de entrada — un leve zoom al abrir el popup, aplicado solo como transform de
  render (`ctx.save/translate/scale/restore` alrededor de los `draw*()`), nunca toca las
  coordenadas reales de la pelota — así que no puede afectar la física ni la detección
  de acierto.

**La física, el rango de arrastre (130px), el ángulo de golpe, y las 3 fichas
(juego → email → código) son exactamente las mismas.** El juego se ve mejor; no juega
distinto.

### Si hay que revertir

El tema publicado no fue modificado — el popup en vivo sigue siendo el original hasta
que se publique este tema. Para revertir solo este archivo dentro del preview, hay que
volver a subir la versión que trae el tema `MAIN` (`211890307325`).

## `sections/page-canvas.liquid` + `templates/page.canvas.json`

Plantilla en blanco para páginas 100% custom (HTML/CSS/JS propio en el body de la
página, sin el layout de bloques del tema). A diferencia de `main-page.liquid` (la
plantilla por defecto), esta **no** envuelve el contenido en `.page-width-content` ni
agrega el `<h1>` automático del título — así una página puede ser full-bleed de verdad.

**Instalado en:** tema `MAIN` (`211890307325`), el publicado. A diferencia de todo lo
demás en esta carpeta, esto se subió directo a producción — son archivos **nuevos**, no
tocan nada existente (ningún template ni página los usaba antes), así que no hay riesgo
de romper algo que ya funcionaba. Verificado contra `assets/base.css`: `.content-for-layout`
no impone ancho ni padding — el límite de ancho del tema (`--page-margin`) solo se aplica
a elementos con clase `.page-width-*`, que esta plantilla no usa.

**Primer uso:** la página `/pages/fundador15` (ver `marketing/2026-08-fundador15-*`).

### Cómo usarla para una página nueva

1. Crear la página en Shopify con `templateSuffix: "canvas"`.
2. El `body` de la página es HTML/CSS/JS libre — se renderiza tal cual, sin pasar por
   Liquid (Shopify no interpreta el contenido de página como Liquid).
3. Sin `<title>` ni `<meta>` en el body — no tienen efecto ahí. El título real de la
   página lo pone el campo `title` de la página misma.

### Si hay que revertir

Son archivos nuevos y aditivos — nada más los referencia. Borrar ambos archivos del tema
no rompe nada existente, solo las páginas que usen `templateSuffix: "canvas"` (volverían
a la plantilla por defecto y perderían el diseño custom).
