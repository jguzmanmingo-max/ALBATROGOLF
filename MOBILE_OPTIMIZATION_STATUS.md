# Optimización Mobile — Estado

**Corte:** 17 agosto 2026 · **Rama:** `claude/audit-skills-mcps-q4sq4q`

Mobile es el 86% del tráfico (598 sesiones en 14 días) y convierte **6× peor** que
desktop: 1,0% de add-to-cart contra 6,4%. Ahí está el freno principal de la tienda.

---

## 1. Lo que ya está aplicado en la tienda

Estos cambios están **en vivo**. No requieren publicar nada.

### 1.1 Guante de cuero — reestructurado

El producto #2 en ingresos tenía las variantes en **un solo campo de texto**, que en
mobile se renderiza como un dropdown de tiras largas imposibles de tocar bien:

| Antes | Ahora |
|---|---|
| `M. Mano Izquierda` | `M / Izquierda` |
| `S. Mano izquierda` | `S / Izquierda` |
| `L mano izquierda` | `L / Izquierda` |
| `M-L. Mano izquierda.Talla intermedia` | `M-L / Izquierda` |
| `M. Mano DERECHA(para zurdos)` | `M / Derecha` |

Ahora son **dos selectores de botones**: `Talla` (S · M · M-L · L) y `Mano`
(Izquierda · Derecha). Se agregaron SKUs normalizados, que antes eran `null`:

| Variante | SKU | Stock |
|---|---|---|
| S / Izquierda | `GUA-CUERO-S-IZQ` | 29 |
| M / Izquierda | `GUA-CUERO-M-IZQ` | 49 |
| M-L / Izquierda | `GUA-CUERO-ML-IZQ` | 19 |
| L / Izquierda | `GUA-CUERO-L-IZQ` | 4 |
| M / Derecha | `GUA-CUERO-M-DER` | 5 |
| | **Total** | **106** ✅ |

> ⚠️ **Las variantes nuevas quedaron en `DENY`** (no vende sin stock). Si quieres
> aceptar pedidos por encargo — que tiene sentido, porque produces los guantes tú —
> hay que cambiarlas a `CONTINUE`. Es una decisión tuya, no la tomé.

### 1.2 Pack de 2 guantes — reestructurado

Mismo problema. Su opción estaba **vinculada a la taxonomía estándar de Shopify**
(`shopify.accessory-size`), que no acepta valores libres, así que hubo que
desvincularla y rehacerla como opción propia.

`S` · `M` · `M-L` como botones cortos. SKUs `PACK2-GUA-S/M/ML`. Stock intacto:
M = 10, S = 0, M-L = 0. Se preservó `inventoryPolicy: CONTINUE` tal como estaba.

> ⚠️ Con `CONTINUE` y stock 0, las tallas S y M-L **se pueden comprar sin existir**.
> Eso es lo que produjo el stock en −1 que aparecía en el informe. Decide si quieres
> venta por encargo (dejar `CONTINUE`) o mostrar "Agotado" (cambiar a `DENY`).

### 1.3 Poleras — consolidadas

Había **3 fichas para 1 producto**. En mobile eso obliga a salir y comparar en pestañas.

- `9290826809597` (solo verde, 4 tallas) → **ARCHIVADA**
- `9290840899837` (solo navy, 2 tallas) → **ARCHIVADA**
- `15290872365309` → **única ficha**, con `Color` (Verde Petróleo · Azul Navy) × `Talla`

Quedó en 2 variantes agotadas (`Verde Petróleo / M`, `Azul Navy / M`) según tu
instrucción, hasta que compres stock. Cuando llegue, hay que volver a agregar las tallas.

### 1.4 Texto alternativo de imágenes — 9 corregidos

El `alt` es lo que se ve mientras carga la imagen en conexión lenta, y es lo que lee
Google. Estaban vacíos o **con el producto equivocado**:

| Producto | `alt` antes | `alt` ahora |
|---|---|---|
| Guante adulto | «Guante de golf **Junior**…» ❌ | «Guante de golf de cuero premium Albatro Golf Players Collection» |
| Guante (palma, dorso, empaque) | vacío | descripción de cada vista |
| Pack 2 guantes | vacío | «Pack de 2 guantes de golf de cuero premium Albatro Golf» |
| Toalla (3 fotos) | vacío | descripción de cada vista |
| Creatina gummies | **«Gray helmet for bikers»** ❌ | «Creatine Monohydrate 30 gummies — frente del envase» |

El «Gray helmet for bikers» era dato de muestra de Shopify que quedó filtrado en el catálogo.

### 1.5 Títulos — 11 corregidos

Un título largo hace *wrap* a 3 líneas en celular y **empuja el botón de compra bajo
el fold**. Y en una página de colección, mayúsculas y minúsculas mezcladas hacen ver
la tienda amateur.

| Antes | Ahora |
|---|---|
| `Guante de golf 100%␣␣Albatro Golf Players collection.Cuero premium.` (68 car.) | `Guante de Golf de Cuero Premium — Players Collection` (51) |
| `Pack de 2 guantes de cuero premium Albatro golf Players collection` | `Pack de 2 Guantes de Cuero Premium — Players Collection` |
| `Guante de golf Junior Albatro Golf – Cuero premium` | `Guante de Golf Junior — Cuero Premium` |
| `Toalla de Golf Albatro Golf con Logo Bordado + llavero retráctil` | `Toalla de Golf con Logo Bordado + Llavero Retráctil` |
| `Marcador de Pelota metálico,Albatro Golf` | `Marcador de Pelota Metálico Albatro Golf` |
| `Lapiz de golf` | `Lápiz de Golf Albatro` |
| `FAJA LUMBAR` | `Faja Lumbar de Compresión` |
| `hombrera soporte ortopédica` | `Hombrera Soporte Ortopédica` |
| `RODILLERA CON GEL␣␣FRIO Y CALOR` | `Rodillera con Gel Frío y Calor` |
| `CREATINE MONOYDRATE 30 GUMMIES` | `Creatina Monohidratada — 30 Gummies` |
| `Trx - Bandas` | `Kit de Entrenamiento TRX con Bandas` |
| `Life Extension Nad` | `Life Extension NAD+` |

**Los handles (URLs) no se tocaron.** Ningún link existente se rompe.

---

## 2. Lo que está esperando que publiques

Hay un tema duplicado y **sin publicar** con la barra fija de "Agregar al carrito"
para mobile ya instalada y funcionando:

**Tema:** `MOBILE FIX — sticky ATC + swatches (PUBLICAR ESTA)`
**ID:** `212411449597`

**Previsualízalo antes de publicar:**
```
https://www.albatrogolf.cl/products/guante-de-golf-100-cuero-produccion-propia?preview_theme_id=212411449597
```
Ábrelo en el celular, baja hasta que el botón original salga de pantalla, y la barra
debe aparecer abajo. Prueba agregar al carro con ella.

**Si te gusta:** Tienda en línea → Temas → en ese tema, `···` → **Publicar**.
**Si no:** bórralo. El tema actual sigue publicado e intacto, no se tocó nada en él.

### Por qué hacía falta código y no un toggle

Tu tema es **Horizon**, y tiene `sticky_details_desktop: true` — o sea, sticky
**solo en desktop**. Horizon no trae sticky add-to-cart para mobile, así que no existe
un switch que activar. Hay que agregarlo.

### El intento anterior y por qué este es distinto

Encontré dos reglas en el código apagando barras sticky previas:

```css
/* snippets/albatro-effects.liquid */
.ag-atc-bar { display: none !important; }          /* "barra sticky sin funcionalidad" */

/* snippets/albatro-init.liquid */
.sticky-add-to-cart { display: none !important; }  /* "tapaba descripciones y mostraba Agotado erroneo" */
```

Alguien ya intentó esto, salió mal, y lo apagaron con CSS en vez de arreglarlo.
Las tres quejas están resueltas en `snippets/albatro-sticky-atc.liquid`:

| Queja anterior | Cómo se resolvió |
|---|---|
| «sin funcionalidad» | El botón de la barra hace `click()` sobre el botón real de la ficha. No duplica lógica: reutiliza la selección de variante, el AJAX y el cart drawer del tema. |
| «mostraba Agotado erróneo» | Espeja el estado real del botón (`disabled`, etiqueta) y se re-sincroniza cuando el tema re-renderiza el formulario al cambiar de variante. |
| «tapaba descripciones» | Solo aparece cuando el botón real **ya salió de pantalla** (`IntersectionObserver`), y agrega padding al `body` para no tapar el último bloque. |

Además: usa la clase `.albatro-sticky-atc`, que **no colisiona** con las dos reglas de
arriba; se esconde sola cuando hay un `dialog` abierto (para no tapar el cart drawer);
se mueve al `<body>` al iniciar para que `position: fixed` se ancle al viewport; área
táctil de 48 px; respeta `safe-area-inset-bottom` (notch de iPhone); y si no encuentra
el botón real **no renderiza nada** — falla en silencio, nunca rompe la página.

Archivos tocados en el tema duplicado, nada más:
- `snippets/albatro-sticky-atc.liquid` — nuevo
- `layout/theme.liquid` — +2 líneas al final del `<body>` (5.199 → 5.343 bytes, verificado)

---

## 3. El checklist del informe, resuelto

Leí la configuración real de tu tema. Cuatro de los seis puntos **ya estaban bien**:

| # | Punto del informe | Estado real |
|---|---|---|
| 1 | Botón "Agregar al carrito" sticky en mobile | 🔴 No existía → **resuelto en el tema sin publicar** |
| 2 | Velocidad / comprimir imágenes | ⚪ Pendiente — hay que correr el test desde el admin |
| 3 | Selectores como botones táctiles, no dropdown | 🟢 **Ya estaba**: `variant_style: "buttons"` |
| 4 | Revisar apps activas | ⚪ No pude leerlas (fuera del alcance del token) |
| 5 | Checkout acelerado | 🟢 El bloque `accelerated-checkout` **ya está** en el tema. Falta confirmar que Shop Pay / Google Pay / Apple Pay estén activos en Configuración → Pagos. |
| 6 | Fotos completas sin recortes raros | 🟢 **Ya estaba**: `media_fit: "contain"`, `aspect_ratio: "adapt"` |

Así que del checklist original solo te quedan **dos cosas reales**: el test de
velocidad (#2) y confirmar los medios de pago (#5). Los demás estaban resueltos o
quedaron resueltos acá.

---

## 4. Lo que queda y necesita tu decisión

- [ ] **Política de inventario.** Guante = `DENY`, Pack = `CONTINUE`. Hay que unificar:
      ¿aceptas pedidos por encargo o muestras "Agotado"? Con `CONTINUE` y stock 0 se
      venden cosas que no existen.
- [ ] **Stock real de la polera.** Las 2 variantes están en 0. Cuando compres, hay que
      volver a cargar tallas y cantidades.
- [ ] **Dos productos "Próximamente" están ACTIVOS con stock 0**
      (`Próximamente — Fitness Golf #1` y `#2`). En mobile son taps desperdiciados.
      Deberían pasar a borrador.
- [ ] **`Recovery #1`** sigue con nombre de placeholder (handle: `kit-recovery-04-rodilla`).
- [ ] **Bloque de inventario para urgencia.** Horizon trae `product-inventory`, que
      muestra "Quedan 4". Con el guante L en 4 unidades y el pack M en 10, es urgencia
      real y gratis. Se agrega arrastrando el bloque en el editor de temas, entre el
      selector de variantes y el botón de compra.
- [ ] **Handles que no coinciden con el título** (no los toqué porque cambiarlos rompe
      URLs): el pack dice `pack-de-3-guantes` (son 2), `Kettlebell 10kg` →
      `kettlebell-12kg`, `Par de Mancuernas 10kg` → `par-de-mancuernas-8kg-c-u`.
- [ ] **Taxonomía sin normalizar.** Conviven `Recovery & Health`, `Salud`, `Bienestar`,
      `Otro`, `DROPI CUP`, `Deportes`, `Hogar`, `Fitness Golf` y productos con
      `productType` vacío.

---

## 5. Incidente — guante roto y reparado

Hay que dejarlo registrado.

Durante un intento anterior de reestructurar el guante en esta misma sesión, se
borraron sus 5 variantes con `productVariantsBulkDelete` **antes** de tener lista la
estructura de reemplazo. Shopify colapsa un producto sin variantes a una sola variante
`Default Title`, y el producto quedó con **1 variante sin nombre y 5 unidades** en vez
de 5 variantes y 106 unidades.

Se reparó con `productSet`, que declara opciones, variantes e inventario en una sola
operación atómica. Las 106 unidades están restauradas y verificadas.

**La lección, para la próxima:** cuando una opción de producto solo necesita nombres
más cortos, se renombra en el lugar con `productOptionUpdate` — preserva IDs de
variante, inventario y ubicaciones. Nunca borrar variantes primero. `productSet` es la
herramienta correcta cuando sí hay que rehacer la estructura, porque es atómica: si
falla, no deja el producto a medias.

---

## 6. Impacto esperado

| Cambio | Efecto estimado en add-to-cart mobile |
|---|---|
| Variantes como botones cortos (guante + pack) | +0,8 a +1,5 pts |
| Barra sticky de compra | +0,3 a +0,6 pts |
| Títulos cortos → botón sobre el fold | +0,2 a +0,4 pts |
| `alt` correcto (carga lenta + SEO) | +0,1 a +0,2 pts |

**De 1,0% a un rango realista de 2,4–3,7%** en una o dos semanas, midiendo después de
publicar el tema. El benchmark de la categoría es 6–10%, así que incluso el techo de
esto sigue dejando espacio.

Medir con:
```sql
FROM sessions SHOW sessions, sessions_with_cart_additions, conversion_rate
GROUP BY device_type SINCE -14d UNTIL today
```
