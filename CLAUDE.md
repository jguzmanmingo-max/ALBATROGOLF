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

### Embudo

| Paso | Valor | Tasa | Benchmark | Estado |
|---|---|---|---|---|
| Sesiones | 4.576 | — | — | — |
| Agregó al carro | 49 | **1,07%** | 6–10% | 🔴 crítico |
| Llegó a checkout | 38 | 77,6% del ATC | 60–80% | 🟢 sano |
| Completó compra | 11 | **28,9%** del checkout | 45–65% | 🔴 malo |
| **Conversión total** | **11** | **0,24%** | 1,4–2% | 🔴 ~6–8× bajo |

**Lectura:** el cuello de botella #1 **no es el checkout, es la ficha de producto.**
98,9% de las visitas no agregan nada al carro. Quien sí agrega, llega a checkout sin problema.

### Dinero

| Métrica | CLP |
|---|---|
| Gross sales | 336.001 |
| Descuentos | **−128.209 (38,2% del gross)** |
| Net sales | 177.802 |
| Total sales | 231.278 |
| AOV | 18.890 |
| Órdenes | 11 |

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

### Lo que sí genera ingreso

Pelotas usadas AAA (77.427 / 3 órdenes, sin descuento), guantes de cuero (36.958 / 2), pack 2 guantes, MIX Bajo Par, toalla, hebilla.

> **Insight estratégico:** vende el **golf core** (pelotas, guantes, accesorios de marca), no el catálogo de recovery/salud de Dropi. El catálogo Dropi ocupa la mayoría de las fichas y aporta casi nada al top 10.

---

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

- [ ] **Verificar config de impuestos.** `taxes` = 30.112 sobre `net_sales` = 162.466 (≈18,5%). Si los precios están cargados como *tax-exclusive*, el cliente ve 9.990 y en checkout le cobran ~11.888 → causa clásica de abandono en Chile y explicaría el 28,9% de completitud. Revisar en Shopify → Configuración → Impuestos.
- [ ] **Boleta electrónica SII.** En Chile es obligatoria por Ley 21.210 desde 2021, también para e-commerce. Shopify **no** tiene integración nativa con el SII — requiere Lioren, Bsale, Nubox u OpenFactura.
- [ ] **Medios de pago locales.** Confirmar Webpay/Transbank y Mercado Pago activos. Su ausencia es otra causa mayor de abandono en checkout.
- [ ] **Auditar los 4 productos con `net_sales = 0`.**
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
