# Correo para los 65 — FUNDADOR15 + envío gratis

**Estado:** borrador listo en Gmail (`jguzmanmingo@gmail.com`), sin enviar. Falta tu revisión final y resolver desde qué casilla sale.
**Enviar antes del:** 24 de agosto (el código vence ese día a las 23:59)
**Lista:** los 65 correos de `albatro-leads-para-escribir.csv` — en copia oculta (BCC), nadie ve al resto.
**Página de canje (interactiva):** https://www.albatrogolf.cl/pages/fundador15 — instalada como página real de la tienda (código con botón de copiar, countdown en vivo, los 3 productos con compra directa).

---

## Por qué se rehizo

La v1 metía todo el contenido (código + 3 productos + condiciones) directo en el cuerpo del
correo — plano, sin diseño, y con una frase floja ("precio de golfista que no le regala nada a
nadie"). Se separó en dos piezas:

1. **El correo** — corto, con un solo gancho y un solo botón. Su trabajo es que lo abran y
   hagan clic.
2. **La página de canje** — ahí vive la experiencia: el código con botón de copiar, countdown
   en vivo al 24 de agosto, y los 3 productos con link directo a comprar (con el código
   pre-aplicado vía `/discount/FUNDADOR15?redirect=...`).

## Asunto (elige uno)

**A** — Llegaste primero. Esto es tuyo.
**B** — Tu código de fundador sigue esperándote

---

## Cuerpo

```
Asunto: Llegaste primero. Esto es tuyo.

Hola,

Jugaste el Putt Albatro, o te anotaste antes de que abriéramos.
Esa espera vale un código que no le dimos a nadie más.

FUNDADOR15 — 15% + envío gratis, sin mínimo.
Se acaba el 24 de agosto a las 23:59.

👉 Ver mi código y el kit de fundador

Nos vemos en la cancha.
Albatro Golf
```

El botón lleva a la página de canje (código + countdown + los 3 productos que más se venden:
guante de cuero, pelotas AAA, pack de 2 guantes).

---

## Notas para ti (José)

1. **La página de canje ya quedó instalada en albatrogolf.cl** (no en preview):
   https://www.albatrogolf.cl/pages/fundador15 — código con botón de copiar, countdown en
   vivo al 24 de agosto, y los 3 productos con compra directa (código pre-aplicado). No pude
   verla renderizada yo mismo — este chat tiene bloqueado el acceso de navegación a
   albatrogolf.cl (política del entorno, no del sitio) — así que verifiqué a mano contra el
   CSS del tema que nada le impone un ancho máximo ni le mete un título duplicado encima, pero
   igual dale una mirada rápida desde tu celular antes de mandar el correo.

2. **Quién manda el correo** — el Gmail conectado en este chat es `jguzmanmingo@gmail.com`, no
   `hola@albatrogolf.cl`. La herramienta de Gmail no deja elegir un remitente distinto al de la
   cuenta conectada, así que el borrador quedó armado ahí (asunto + cuerpo con diseño +
   los 65 en copia oculta), no en `hola@albatrogolf.cl`. Para que salga de esa casilla: (a)
   la conectas en los ajustes de conectores de este chat, o (b) copias el borrador y lo mandas
   tú mismo desde donde administres `hola@albatrogolf.cl`.

3. **Los 3 que ya compraron** (eliasjmc, salvadorcartest, ftorresh61) — verificado de nuevo en
   vivo contra Shopify antes de esta versión, siguen fuera de la lista. Cero duplicados: 65
   filas, 65 correos únicos.

4. **Archivos de referencia** — `2026-08-fundador15-pagina-canje.html` (diseño de la página,
   igual al que quedó instalado salvo el `<title>`/`<meta>` del encabezado, que en Shopify no
   van en el body) y `2026-08-fundador15-email.html` (el HTML exacto del correo). Los archivos
   de tema nuevos (`sections/page-canvas.liquid`, `templates/page.canvas.json`) están
   documentados en `theme/README.md`.
