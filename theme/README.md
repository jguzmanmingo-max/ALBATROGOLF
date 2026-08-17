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
