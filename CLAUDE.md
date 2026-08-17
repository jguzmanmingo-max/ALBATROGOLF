# ALBATROGOLF — Contexto operativo

Este archivo se carga en **toda** sesión de Claude Code sobre este repo.
Su función es que ninguna sesión empiece desde cero.

---

## 1. Qué es este negocio

**Albatro Golf** — tienda de golf online en Chile.

| Dato | Valor |
|---|---|
| Dominio | `www.albatrogolf.cl` (`mfax9u-an.myshopify.com`) |
| Contacto | `hola@albatrogolf.cl` |
| Plataforma | Shopify, plan **Basic** |
| Moneda | **CLP** (peso chileno) |
| Mercado | Chile · zona horaria UTC−4 |
| IVA | **19%** |
| Idioma | Español de Chile (tuteo, no "vosotros", no español neutro-España) |

**Modelo mixto:**
- **Producto propio / marca Albatro** — guantes de cuero, toallas, pelotas usadas grado AAA, accesorios. *Es lo que realmente vende.*
- **Dropshipping vía Dropi** — catálogo de recuperación y salud (rodilleras, kinesio tape, pistolas de masaje, fajas). Etiquetas `dropi`, `fulfillment-droplocal`, `fulfillment-propio`.

> ⚠️ El `totalInventory` de los productos Dropi (1710, 1100, 1000…) es **catálogo del proveedor, no stock propio**. Nunca lo trates como inventario real ni como capital inmovilizado.

---

## 2. Números reales (ventana de 90 días, corte 2026-08-14)

Medidos con ShopifyQL. **Actualizar en cada auditoría** — ver `/auditoria-funnel`.

> ⚠️ **4 de las 11 órdenes son pruebas del dueño.** Confirmado 2026-08-14.
> `#1002` (19 jun, 0 CLP, 4 ítems — el 100% de descuento), `#1003` (3.990), `#1004` (1.990), `#1005` (1.990).
> Todas a nombre de **José María Guzmán Mingo** y todas `UNFULFILLED`.
> **Las 7 reales son `#1006`–`#1012`**, todas `PAID` + `FULFILLED`.
> ShopifyQL **no** las separa. Toda cifra de abajo está corregida a mano.

### Embudo

| Paso | Bruto | Real (sin pruebas) | Benchmark | Estado |
|---|---|---|---|---|
| Sesiones | 4.576 | ~4.572 | — | — |
| Agregó al carro | 49 | ~45 · **0,98%** | 6–10% | 🔴 crítico |
| Llegó a checkout | 38 | ~34 · 75,6% del ATC | 60–80% | 🟢 sano |
| Completó compra | 11 | **7** · ~20,6% del checkout | 45–65% | 🔴 malo |
| **Conversión total** | 0,24% | **0,15%** | 1,4–2% | 🔴 ~10× bajo |

**Lectura:** el cuello de botella #1 **no es el checkout, es la ficha de producto.**
99% de las visitas no agregan nada al carro. Quien sí agrega, llega a checkout sin problema.
El cierre de checkout (~20,6%) es el problema #2 y es peor de lo que parecía antes de descontar las pruebas.

### Dinero

| Métrica | Bruto (11 órdenes) | Real (7 órdenes) |
|---|---|---|
| Gross sales | 336.001 | ~191.071 |
| Descuentos | −128.209 (38,2%) | **~−21.239 (~11%)** 🟢 normal |
| Net sales | 177.802 | ~169.832 |
| Total sales | 231.278 | **227.298** |
| **AOV** | 18.890 | **32.471** 🟢 |

**Dos correcciones importantes:**
1. **La "fuga de descuentos" del 38,2% no existe.** Era la orden de prueba `#1002`. El descuento real (~11%) está en rango normal.
2. **El AOV real es 32.471 CLP, no 18.890.** Las pruebas de 0–3.990 CLP hundían el promedio. Esto **cambia la economía unitaria**: a 32.471 de ticket el tráfico pagado sí puede ser viable, cosa que a 18.890 era casi imposible. Rehacer el cálculo con este número.

### Higiene de datos — pendiente

Las órdenes de prueba quedan en la analítica de Shopify **para siempre**.
Para futuras pruebas usar **Bogus Gateway** (Configuración → Pagos → modo de prueba):
esas órdenes sí quedan excluidas de los reportes. Una orden con 100% de descuento **no** se excluye.

### Tráfico por fuente

| Fuente | Sesiones | Órdenes | Conversión |
|---|---|---|---|
| social | 2.293 | 1 | **0,04%** 🔴 |
| direct | 2.218 | 10 | 0,45% |
| search | **53** | — | SEO inexistente 🔴 |
| email | **1** | — | sin retención 🔴 |

### Anomalía: productos con `net_sales = 0`

136.960 CLP de "ventas" con **cero ingreso neto** — 100% descontado o reembolsado:

| Producto | Gross | Net |
|---|---|---|
| Potencia Rotacional — Kit de Entrenamiento | 44.990 | **0** |
| Protocolo Codo de Golfista | 36.990 | **0** |
| Core & Movilidad — Kit de Entrenamiento | 29.990 | **0** |
| Mini Máquina Masajes Portátil Cabezales | 24.990 | **0** |

Esto explica casi todo el gap gross→net. **Antes de cualquier análisis de rentabilidad, confirmar si fueron regalos/tests intencionales o códigos de descuento fuera de control.**

### Economía unitaria (COGS confirmados 2026-08-14)

| | Precio | COGS | Margen | % |
|---|---|---|---|---|
| Mix 12 Pelotas AAA | 21.990 | 9.600 (12 × 800) | 8.109 | **36,9%** 🟢 |
| Guante de cuero | 21.990 | 9.600 (con packing) | 8.109 | **36,9%** 🟢 |
| Pack 2 guantes | 35.990 | 19.200 | 9.784 | 27,2% 🟡 |

**Blended: 10.384 CLP/orden · 29,5%.**
**CAC breakeven 10.384 · CAC objetivo (20% utilidad) ~5.000 CLP (~USD 5,3).**

→ El tráfico pagado **es viable**, pero solo con la ficha arreglada. Ver `/economia-unitaria`.

### Lo que sí genera ingreso

Pelotas usadas AAA (77.427 / 3 órdenes, sin descuento), guantes de cuero (36.958 / 2), pack 2 guantes, MIX Bajo Par, toalla, hebilla.

> **Insight estratégico:** vende el **golf core** (pelotas, guantes, accesorios de marca), no el catálogo de recovery/salud de Dropi. El catálogo Dropi ocupa la mayoría de las fichas y aporta casi nada al top 10.

---

## 2.b Mercado y competencia (investigado 2026-08-14)

### Tamaño

| Dato | Valor | Variación |
|---|---|---|
| Golfistas federados en Chile | **17.000** | +15% desde 2018 (6.700 nuevos) |
| Rondas oficiales 2025 | **520.000** | +35% |
| Rondas por jugador/año | 32 | +17% |
| Torneos oficiales | 100+ · 200 días/año | +25% |

Fuente: Federación Chilena de Golf (`chilegolf.cl`).
Mercado chico pero de **consumo recurrente** — el crecimiento en rondas importa más que el de jugadores.

### Competencia

| Competidor | Qué es | Polera |
|---|---|---|
| `golfchile.cl` | Rep. oficial Titleist / FootJoy / Vokey / Scotty Cameron | 50.990–72.990 |
| `lackingtongolf.cl` | Tienda chilena — palos, ropa, accesorios | — |
| Falabella · Ripley | Categoría golf en retail masivo | — |
| MercadoLibre Chile | Head covers genéricos importados | — |

> **Hueco de mercado: no existe una marca chilena de golf.** Todo se surte de etiqueta importada
> a precio de importación. Ahí es donde Albatro ya está ganando con guantes y pelotas.

### Línea nueva en desarrollo — precios sugeridos

⚠️ COGS **estimados**. Reemplazar por los reales y rehacer con `/economia-unitaria`.

| Producto | Precio | COGS est. | Margen | % |
|---|---|---|---|---|
| Polera técnica | 39.990 | 12.000 | 20.205 | **50,5%** |
| Head cover maderas | 16.990 | 4.500 | 9.182 | **54,0%** |
| Toalla pro | 14.990 | 3.500 | 8.572 | **57,2%** |

Los tres superan el 36,9% de pelotas y guantes.
Complementos sugeridos sin talla: marcadores de bola (4.990), tees (3.990), jockey (24.990).

### Palanca de carrito pendiente

**Envío gratis sobre 39.990** (+23% sobre el AOV de 32.471). Cualquier producto sobre
8.400 cubre los 3.100 de despacho al margen del 36,9% — el umbral se financia solo.
Requiere primero tener catálogo combinable.

## 3. Stack conectado

**MCP activos en chat:** Shopify · MCP META (Meta Ads) · Motion Creative Analytics · Canva · Adobe for Creativity · Higgsfield (imagen/video/voz) · Lovable · Google Drive

**Instalados pero APAGADOS en chat** (activar en ajustes de conectores): Gmail · Notion · Microsoft 365 · Cloudflare · Base44

**Sin conectar, relevantes:** Klaviyo (email/SMS) · Stripe · analítica web

---

## 4. Reglas de trabajo

### Dinero
1. **Nunca publiques ni cambies un precio sin pasar por `/economia-unitaria`.** Con 38% de descuento y COGS de dropshipping, el margen puede ser negativo.
2. Precios en CLP **sin decimales**, terminados en `.990` (convención chilena): `9.990`, `12.990`, `19.990`.
3. Los precios al consumidor en Chile se muestran **IVA incluido**. Para margen: `neto = precio / 1,19`.
4. Todo descuento >15% necesita justificación explícita del usuario.

### Catálogo
5. **Taxonomía**: hoy conviven `Recovery & Health`, `Salud`, `Otro`, `DROPI CUP`. Normalizar a español, un solo esquema. No inventes categorías nuevas.
6. **SKU**: hay SKUs que son frases (`Mini Pistola De Masajes`, `Pack 2 Rodillera De Compresion Ajustable`). Formato objetivo: `CAT-PRODUCTO-VARIANTE` en mayúsculas, sin espacios.
7. Nunca pases un producto de `DRAFT` a `ACTIVE` sin ficha completa (ver `/ficha-producto`) — hay drafts con inventario alto sin publicar.
8. Varias imágenes vienen del CDN de MercadoLibre (`D_NQ_NP_*`) o son generadas (`Gemini_Generated_Image_*`). Marcar para reemplazo por foto propia.

### Escritura
9. **Toda la conversación con el usuario va en español.** Respuestas, resúmenes, preguntas, mensajes de PR y comentarios en GitHub. No cambiar a inglés aunque el código, los logs, las herramientas o la documentación técnica estén en inglés.
10. Español de Chile, tuteo, directo. Sin relleno de marketing genérico.
11. El cliente es **golfista**, no paciente. El beneficio se expresa en términos de juego (swing, ronda, hoyo), no clínicos.

### Antes de tocar Shopify
11. Lectura libre. **Toda escritura** (precio, estado, inventario, descuento) se confirma con el usuario primero.

---

## 5. Pendientes conocidos

- [x] ~~**Verificar config de impuestos.**~~ **RESUELTO 2026-08-14.** Verificado orden por orden: `#1007`–`#1012` cobran **IVA incluido** (correcto para Chile). Solo `#1006` sumó el IVA encima (21.990 + 4.178 = 29.268 pagados). Se corrigió entre el 27 y 28 de julio. Si reaparece, es regresión.
- [ ] **El catálogo Dropi vendió CERO unidades a clientes reales en 90 días.** Las 7 órdenes reales son 100% golf core de marca propia (pelotas, guantes, toalla, hebilla, lápiz). Los ~20 productos de recuperación/salud ocupan la mayoría de las fichas y no aportan nada. Decidir: despublicar, o rehacer fichas y probar de verdad.
- [ ] **Dos descuentos sobre el límite del 15%:** `#1011` (10.000 sobre 34.990 = 28,6%) y `#1012` (8.500 sobre 35.990 = 23,6%). Justo en los dos tickets más altos.
- [ ] **Boleta electrónica SII.** En Chile es obligatoria por Ley 21.210 desde 2021, también para e-commerce. Shopify **no** tiene integración nativa con el SII — requiere Lioren, Bsale, Nubox u OpenFactura.
- [ ] **Medios de pago locales.** Confirmar Webpay/Transbank y Mercado Pago activos. Su ausencia es otra causa mayor de abandono en checkout.
- [ ] **Auditar los 4 productos con `net_sales = 0`.**
- [x] ~~**La bio de Instagram manda a `/pages/lista-espera`.**~~ **RESUELTO por el dueño**
  antes del 2026-08-17. En los últimos 14 días esa página recibe **0 sesiones** de
  Instagram; el tráfico entra al home (182) y a fichas. El dato de 874 sesiones era el
  agregado de 90 días y arrastraba el problema viejo.
  ⚠️ La página **sigue publicada** y sigue diciendo «Albatro abre el 28 de julio».
  Conviene despublicarla o reescribirla.
- [ ] 🔴 **74 correos sin tocar.** Los 74 clientes de la tienda están etiquetados
  `lista-espera`, **todos `SUBSCRIBED`**, y prácticamente ninguno compró.
  Entraron por **dos** formularios, ambos `form_type=customer` de Shopify:
  1. `/pages/lista-espera` → tag `lista-espera`
  2. El popup **«El Putt Albatro»** (`snippets/albatro-popup-golf.liquid`), un minijuego
     de putt a los 15 s de navegación → tags `popup-golf,lista-espera`
  **Shopify no notifica al comerciante los registros de newsletter** — solo órdenes.
  Por eso nunca llegó un aviso: quedaron en Clientes, sin que nada los tocara.
  Sin plataforma de email conectada, nadie les escribió nunca.
- [ ] 🔴 **`FUNDADOR15` se usó UNA vez, y no es porque esté roto.**
  Config verificada: todos los clientes, sin mínimo, toda la tienda, sin vencimiento,
  `ACTIVE` desde el 2026-06-11. La orden `#1009` lo usó de punta a punta, así que el
  mecanismo funciona. Las dos causas reales:
  1. **El código se muestra una sola vez en pantalla y nunca se envía por correo.**
     El popup lo enseña tras dejar el email y marca `state.done` en `localStorage`;
     si no lo copian en ese momento, no hay forma de recuperarlo.
  2. **Lo canibalizan códigos hechos a mano que dan más.** `salvadorcartest@gmail.com`
     tiene tags `popup-golf` — o sea ya tenía su 15% — y compró en `#1011` con
     `CODIGOSALVADOR`: **10.000 sobre 29.940 = 25%**. `#1012` usó `bernardo2`:
     8.500 sobre 30.590 = 21,7%. El 15% automático es la peor oferta de la mesa.
- [ ] 🔴 **Descuentos sin control: 7 códigos activos a la vez.**
  | Código | Título | Usos |
  |---|---|---|
  | `ALBATRO10` | «Bienvenida **15%** · mín $50.000» — el nombre dice 15, el código dice 10 | 0 |
  | `ALBATRO15` | «15% primera compra (**legacy · no usar**)» — activo pese al propio título | 0 |
  | `FUNDADOR15` | Fundador 15% toda la tienda | 1 |
  | `FUNDADOR20` | «Acceso Fundador — Guante» · **20%, sobre el límite de la regla** | 0 |
  | `CODIGOSALVADOR` · `ELIAS20` · `bernardo2` | códigos personales, activos sin vencimiento | 1 · 0 · 1 |
  Ninguno tiene fecha de término. Hay que decidir cuáles sobreviven y ponerles tope.
- [ ] **Cero recuperación de carro abandonado.** 38 llegaron a checkout, 11 compraron. 27 abandonos sin ningún email.
- [ ] `index.html` en la raíz del repo es un stub sin usar (`<!-- Aquí va el código de Shopify -->`). Decidir: borrarlo o convertirlo en landing real.

---

## 6. Skills de este proyecto

| Skill | Para qué |
|---|---|
| `/auditoria-funnel` | Autopsia semanal de embudo + márgenes. Detecta lo de arriba automáticamente. |
| `/economia-unitaria` | Margen real por producto/orden. Precio mínimo viable. CAC máximo pagable. |
| `/ficha-producto` | Ficha de producto en español optimizada a conversión + schema. Ataca el 1,07%. |
| `/seo-chile` | Plan de contenido y keywords para búsqueda en Chile. Ataca las 53 sesiones. |
| `/creativo-meta` | Ciclo cerrado: Motion (qué funciona) → Higgsfield/Canva (producir) → Meta (publicar). |

---

## 7. Notas técnicas de ShopifyQL

Consultas **verificadas** contra esta tienda:

```sql
FROM sales SHOW orders, gross_sales, net_sales, total_sales, average_order_value SINCE -90d UNTIL today
FROM sessions SHOW sessions, sessions_with_cart_additions, sessions_that_reached_checkout, sessions_that_completed_checkout, conversion_rate SINCE -90d UNTIL today
FROM sales SHOW gross_sales, discounts, net_sales, taxes, total_sales, orders GROUP BY order_referrer_source ORDER BY total_sales DESC SINCE -90d UNTIL today
FROM sessions SHOW sessions GROUP BY referrer_source ORDER BY sessions DESC SINCE -90d UNTIL today
FROM sales SHOW gross_sales, net_sales, orders GROUP BY product_title ORDER BY gross_sales DESC LIMIT 10 SINCE -90d UNTIL today
```

⚠️ La columna `sales_reversals` **no existe** en esta tienda — la consulta falla. Usar `gross_sales − discounts − net_sales` para inferir devoluciones.
