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

**Política de inventario: `CONTINUE`** en las 5 variantes — decidido por el dueño el
17 ago 2026. Se vende **por encargo**: la ficha sigue comprable aunque el stock llegue
a 0. Tiene sentido porque los guantes son de producción propia.

### 1.2 Pack de 2 guantes — reestructurado

Mismo problema. Su opción estaba **vinculada a la taxonomía estándar de Shopify**
(`shopify.accessory-size`), que no acepta valores libres, así que hubo que
desvincularla y rehacerla como opción propia.

`S` · `M` · `M-L` como botones cortos. SKUs `PACK2-GUA-S/M/ML`. Stock intacto:
M = 10, S = 0, M-L = 0.

**Política de inventario: `CONTINUE`** — ya estaba así y se mantuvo. Coincide con la
decisión del guante: venta por encargo. Esto explica el stock en −1 que aparecía en el
informe: no era un bug, era una venta por encargo funcionando como corresponde.

### 1.3 Poleras — consolidadas

Había **3 fichas para 1 producto**. En mobile eso obliga a salir y comparar en pestañas.

- `9290826809597` (solo verde, 4 tallas) → **ARCHIVADA**
- `9290840899837` (solo navy, 2 tallas) → **ARCHIVADA**
- `15290872365309` → **única ficha**, con `Color` (Verde Petróleo · Azul Navy) × `Talla`

Quedó en 2 variantes agotadas (`Verde Petróleo / M`, `Azul Navy / M`) según tu
instrucción, hasta que compres stock. Cuando llegue, hay que volver a agregar las tallas.

### 1.4 Texto alternativo de imágenes — 29 corregidos

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

Y otros datos simplemente equivocados:

| Producto | `alt` antes | `alt` ahora |
|---|---|---|
| Par de Mancuernas **10 kg** | «Par de Mancuernas **9 kg** c/u» | «Par de mancuernas de 10 kg cada una» |
| Mancuernas hexagonales **5 kg** | «…Hexagonales **4 kg** c/u» | «…hexagonales de 5 kg cada una» |
| Vaso Térmico **510 ml** | «Vaso Térmico de **500 ml**…» | «…de acero inoxidable de 510 ml con pantalla» |
| Kettlebell 8 kg **y** 10 kg | «Kettlebell **12 kg**» (una imagen compartida entre los dos, etiquetada con un tercer peso) | «Kettlebell de hierro fundido» (genérico, porque la imagen sirve a ambos) |

Al cierre: **cero imágenes sin `alt`** en los 79 productos activos.

### 1.5 Títulos — 44 corregidos

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
| `12 pelotas premium USADAS grado A( Con detalles mínimos)` | `12 Pelotas de Golf Usadas — Grado A (con detalles mínimos)` |
| `50 Uds. Tees de Golf profesionales, sistema en T de plástico de 70mm` (71 car.) | `50 Tees de Golf Profesionales — Sistema en T 70 mm` (50) |
| `Llave de Ajuste Torx T25 para Palos de Golf con Mango en T` | `Llave Torx T25 para Palos de Golf` |
| `Clip para gorras mágnetico` (mal escrito) | `Clip Magnético para Gorras` |
| `Codera Elastica` | `Codera Elástica` |
| `Vendaje Termico Masajeador Con Vibracion` | `Vendaje Térmico Masajeador con Vibración` |
| `MAGNESIO COMPLEX 8 ELEMENTAL 1000 mg` | `Magnesio Complex 8 Elemental 1000 mg` |
| `Pack 5 marcas de golf marca Albatro golf` | `Pack 5 Marcadores de Golf Albatro` |
| `Recovery #1` (placeholder) | `Kit Recovery — Rodilla` |
| `Banda de Resistencia de Tela Golf — Light / Medium / Heavy (individual) colores` (78) | `Banda de Resistencia de Tela — Light / Medium / Heavy` (52) |
| `2 Pesas de Tobillo Ajustable 2,5 kg Pro— Rehabilitación de Rodilla` | `2 Pesas de Tobillo Ajustables 2,5 kg — Rehabilitación de Rodilla` |

…más 20 correcciones de acentuación y mayúsculas del mismo tipo (`Llavero de golf` →
`Llavero de Golf Albatro`, `Compresa de hielo` → `Compresa de Hielo`,
`Masajeador Con Terapia De Luz Roja` → `Masajeador con Terapia de Luz Roja`, etc.).

**Los handles (URLs) no se tocaron.** Ningún link existente se rompe.

### 1.6 Otros arreglos de catálogo

- **`productType` vacío en 4 productos de golf** → asignados: los tees, la llave Torx y
  la toalla chica a `Accesorios Golf`; las pelotas grado A a `Pelotas Usadas`. Sin
  categoría no aparecían en filtros ni en colecciones automáticas.
- **Opciones con nombre en minúscula**, que se leen mal en el selector:
  `color` / `amarillo` → `Color` / `Amarillo`; `rodillera` → `Talla`. Renombradas en el
  lugar con `productOptionUpdate`, sin tocar variantes ni inventario.
- **Tallas de la rodillera estaban en orden `M · L · S · XL`** → reordenadas a
  `S · M · L · XL`. Nadie busca su talla en orden aleatorio.
- **Los dos productos «Próximamente — Fitness Golf #1 y #2»** estaban `ACTIVE` con stock
  0 → pasados a `DRAFT`. En mobile eran taps desperdiciados.
- **Guante Junior** → `CONTINUE` (misma línea de producción propia) y SKU
  `GUA-CUERO-JUNIOR`; antes era `null`.

### 1.7 Los dos packs de tees, ahora distinguibles

No eran duplicados: uno es de **bambú** y el otro de **plástico**. El problema era que
el material solo aparecía en la descripción, así que en la grilla de una colección se
leían igual y había que abrir cada ficha para saber cuál era cuál.

| Antes | Ahora |
|---|---|
| `Pack de 50 Tees de Golf` | `Pack de 50 Tees de Golf — Bambú` |
| `50 Uds. Tees de Golf profesionales, sistema en T de plástico de 70mm` | `Pack de 50 Tees de Golf — Plástico, Sistema en T 70 mm` |

### 1.8 MIX Bajo Par — el cliente elige la talla del guante

El bundle ofrecía `XS · S · M · L · XL`, pero **XS y XL no existen en tu línea de
guantes**: alguien podía comprar una talla que no se puede armar.

Ahora ofrece exactamente las tallas del guante: **`S · M · M-L · L`**, todas en
`CONTINUE` para que ninguna talla bloquee la venta. La opción se renombró de `Talla` a
**`Talla del guante`** — en un pack que también trae pelotas y tees, un selector que
solo dice "Talla" es ambiguo. Se agregaron SKUs `MIX-BAJOPAR-S/M/ML/L`.

Se reaprovechó el slot de XS como `M-L`, conservando sus 2 unidades. **XL se eliminó y
con él sus 2 unidades: el total bajó de 9 a 7.** Como el stock de un bundle depende de
lo que puedas armar y no de la talla, conviene que fijes el número real.

> ⚠️ **El bundle solo sirve para diestros.** El guante suelto tiene `Mano`
> (Izquierda · Derecha), pero el MIX no — asume mano izquierda. Un zurdo no puede
> comprarlo. Si quieres cubrirlo, hay que agregarle una segunda opción `Mano`.

### 1.9 Handles corregidos — 13, todos con redirección

Trece URLs contradecían su producto. La peor: **el pack de 2 guantes vivía en
`/products/pack-de-3-guantes-de-cuero-premium`.**

Todos se cambiaron con `redirectNewHandle: true`, así que **Shopify dejó una redirección
automática desde la URL vieja** — nada se rompe si esos links están en Meta Ads, en el
catálogo de productos o indexados en Google.

| Producto | Handle viejo | Handle nuevo |
|---|---|---|
| Pack de **2** Guantes | `pack-de-3-guantes-de-cuero-premium` | `pack-de-2-guantes-de-cuero-premium` |
| Kettlebell **10** kg | `kettlebell-12kg` | `kettlebell-10kg` |
| Par de Mancuernas **10** kg | `par-de-mancuernas-8kg-c-u` | `par-de-mancuernas-10kg` |
| Mancuernas hexagonales **5** kg | `par-de-mancuernas-4kg-c-u` | `par-de-mancuernas-hexagonales-5kg` |
| Medio Balón Cojín | `disco-de-equilibrio-inflable` | `medio-balon-cojin-masajeador` |
| Banda de Resistencia de Tela | `set-de-3-bandas-de-resistencia-golf` | `banda-de-resistencia-de-tela` |
| Set de Bandas de Resistencia | `set-bandas-de-resistencia-fitness-golf` | `set-de-bandas-de-resistencia` |
| Creatina Gummies | `creatine-monoydrate-30-gummies` (mal escrito) | `creatina-monohidratada-30-gummies` |
| Vaso Térmico **510** ml | `vaso-termico-de-500ml-con-pantalla` | `vaso-termico-acero-inoxidable-510ml` |
| Clip Magnético | `clip-para-gorras-magnetico` (mal escrito) | `clip-magnetico-para-gorras` |
| Tees de plástico | `50-uds-tees-…-de-plastico-de-70mm` (66 car.) | `pack-50-tees-golf-plastico` |
| Tees de bambú | `pack-de-50-tees-de-golf` | `pack-50-tees-golf-bambu` |
| Kit Recovery — Rodilla | `kit-recovery-04-rodilla` | `kit-recovery-rodilla` |

Verificado con `urlRedirects`: las 13 redirecciones existen.

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

- [x] ~~**Política de inventario.**~~ **RESUELTO 17 ago 2026:** guante, pack y Junior en
      `CONTINUE`. Venta por encargo.
- [x] ~~**Badge de despacho 3–5 días.**~~ **Revisado 17 ago 2026:** el dueño confirmó
      que tiene stock de guantes, así que el plazo se cumple. No se toca.
- [x] ~~**Productos "Próximamente" activos.**~~ Pasados a `DRAFT`.
- [x] ~~**`Recovery #1` con nombre placeholder.**~~ Renombrado a `Kit Recovery — Rodilla`.
- [ ] **Stock real de la polera.** Las 2 variantes están en 0 y en `DENY`, así que
      muestran "Agotado" (esto es a propósito, según tu instrucción). Cuando compres,
      hay que volver a cargar tallas y cantidades.
- [x] ~~**Posible duplicado de tees.**~~ **RESUELTO:** no eran duplicados, uno es de
      bambú y el otro de plástico. Ahora el material está en el título.
- [x] ~~**Foto de la Hebilla en la toalla chica.**~~ Revisado con el dueño: se queda.
- [x] ~~**`MIX Bajo Par` con talla `XS`.**~~ **RESUELTO:** ahora ofrece las tallas del
      guante (`S · M · M-L · L`).
- [ ] **Stock real del MIX Bajo Par.** Quedó en 7 unidades después de sacar XL, pero el
      stock de un bundle depende de cuántos puedas armar. Fija el número real.
- [ ] **El MIX Bajo Par solo sirve para diestros.** No tiene opción `Mano`. Si quieres
      vender a zurdos, hay que agregarla.
- [ ] **Bloque de inventario para urgencia.** Horizon trae `product-inventory`, que
      muestra "Quedan 4". Con el guante L en 4 unidades y el pack M en 10, es urgencia
      real y gratis. Se agrega arrastrando el bloque en el editor de temas, entre el
      selector de variantes y el botón de compra.
      ⚠️ Ojo con la venta por encargo: el bloque muestra **"En stock"** cuando la
      cantidad es 0 y la política es `CONTINUE` (así está escrito en
      `blocks/product-inventory.liquid`). O sea que las tallas del pack en 0 dirían
      "En stock". Es coherente con vender por encargo, pero decide si te sirve.
- [x] ~~**Handles que no coinciden con el título.**~~ **RESUELTO 17 ago 2026:** los 13
      corregidos con redirección. Detalle en §1.9.
- [ ] **Taxonomía fragmentada.** 12 `productType` distintos para 79 productos:
      `Recovery & Health`, `Salud`, `Bienestar`, `Otro`, `DROPI CUP`, `Deportes`,
      `Hogar`, `Fitness Golf`, `Nutrición e Hidratación`, `Accesorios Golf`,
      `Pelotas Usadas`, `Marca Albatro`. Ya no hay ninguno vacío, pero `Salud` /
      `Bienestar` / `Recovery & Health` son la misma cosa en tres idiomas de
      clasificación, y `DROPI CUP` es nombre de proveedor, no de categoría. Normalizar
      requiere que decidas el esquema — por eso no lo hice solo.

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
| Catálogo sin faltas de ortografía ni mayúsculas rotas | difícil de aislar, pero la confianza es lo que sostiene el resto |

**De 1,0% a un rango realista de 2,4–3,7%** en una o dos semanas, midiendo después de
publicar el tema. El benchmark de la categoría es 6–10%, así que incluso el techo de
esto sigue dejando espacio.

Medir con:
```sql
FROM sessions SHOW sessions, sessions_with_cart_additions, conversion_rate
GROUP BY device_type SINCE -14d UNTIL today
```
