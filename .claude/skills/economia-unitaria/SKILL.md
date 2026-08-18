---
name: economia-unitaria
description: Calcula el margen de contribución real por producto y por orden en Albatro Golf — precio menos IVA 19%, COGS, comisión de pasarela, envío y CAC. Úsala antes de fijar o cambiar cualquier precio, antes de aprobar un descuento, al evaluar si un producto Dropi conviene, o al decidir cuánto se puede pagar por una venta en Meta Ads. Responde "¿esto deja plata?" y "¿cuál es el precio mínimo viable?".
---

# Economía unitaria — Albatro Golf

Una venta con margen negativo escala la pérdida. Esta skill existe para que eso no pase.

## La cascada (CLP, Chile)

En Chile el precio al consumidor se muestra **IVA incluido**. Se descompone así:

```
Precio de lista (IVA incl.)          P
− IVA 19%                            P − P/1,19   →  neto = P / 1,19
− Descuento aplicado                 d × neto
− COGS (costo Dropi o costo propio)  C
− Comisión pasarela                  ~3,5% del cobro bruto (Webpay/Shopify Payments)
− Envío no cubierto por el cliente   E
─────────────────────────────────────────────────
= Margen de contribución             M   (antes de CAC)

− CAC (gasto publicitario / órdenes) A
─────────────────────────────────────────────────
= Margen neto por orden              M − A
```

### Parámetros verificados (7 órdenes reales, corte 2026-08-14)

| Parámetro | Valor real | Nota |
|---|---|---|
| AOV (total pagado) | **32.471** | No 18.890 — ese incluía pruebas |
| Mercadería neta de IVA por orden | **~24.460** | Después de descuento |
| Envío | **Lo paga el cliente: 3.100** | No subsidiado. Solo #1011 cobró 4.950. |
| Descuento real | **11,1%** | Sobre mercadería |
| IVA | **Incluido en el precio** | Verificado en `#1007`–`#1012` |
| Comisión pasarela | ~3,5% del cobro | ~1.136/orden |

> ⚠️ Solo `#1006` cobró IVA **encima** del precio (21.990 + 4.178). Corregido desde `#1007`.
> Si vuelve a aparecer una orden así, es una regresión en Configuración → Impuestos.

### COGS confirmados por el dueño (2026-08-14)

| Producto | COGS | Origen |
|---|---|---|
| **Pelota usada AAA** | **800 c/u** | Se compran por docena → docena = 9.600 |
| **Guante de cuero** | **9.600** | Ya incluye packing |
| Pack 2 guantes | 19.200 | 2 × 9.600 |
| MIX Bajo Par | ⚠️ **pendiente** | Preguntar contenido del pack |
| Toalla, lápiz, hebilla | ⚠️ pendiente | |

### Márgenes calculados

| | Pelotas 12 AAA | Guante cuero | Pack 2 guantes |
|---|---|---|---|
| Precio lista | 21.990 | 21.990 | 35.990 |
| Neto de IVA (`/1,19`) | 18.479 | 18.479 | 30.244 |
| − COGS | −9.600 | −9.600 | −19.200 |
| − Comisión 3,5% | −770 | −770 | −1.260 |
| **= Margen** | **8.109** | **8.109** | **9.784** |
| **% sobre lista** | **36,9%** 🟢 | **36,9%** 🟢 | **27,2%** 🟡 |

**Blended real** (5 de 7 órdenes calculables): **10.384 CLP/orden · 29,5%**

### CAC pagable

| | CLP | USD aprox. |
|---|---|---|
| Breakeven (utilidad cero) | 10.384 | ~11 |
| **Objetivo — 20% de utilidad** | **~5.000** | **~5,3** |

**Veredicto: el tráfico pagado es viable, pero exigente.** Un CPA de compra de USD 5,3
en Chile es alcanzable con CPMs locales, pero **solo con la ficha arreglada**. Con
add-to-cart en 0,98% el CPA real sería catastrófico. Ficha primero, tráfico después.

### Precio mínimo viable — pelotas

Para sostener un CAC de 5.000 con 20% de utilidad:

```
P_min = 22.912  →  redondeo .990  →  22.990
```

A los 21.990 actuales, con CAC de 5.000, la utilidad queda en **16,8%**.
Subir a 22.990 (+4,5%) devuelve el margen al 20% sin tocar nada más.

### ⚠️ Lo que cuesta un descuento — caso real `#1012`

| Pack 2 guantes | Sin descuento | Con los 8.500 aplicados |
|---|---|---|
| Cobrado | 35.990 | 27.490 |
| Margen | **9.784** | **2.939** |
| % | 27,2% | **10,7%** |

**Ese descuento se comió el 70% del margen.** Por eso el límite del 15% no es decorativo.

### ⚠️ Calcula sobre lo que vende, no sobre el catálogo

**Las 7 órdenes reales son 100% golf core de marca propia. El catálogo Dropi de
recuperación vendió CERO unidades a clientes reales en 90 días.**

Mix 12 Pelotas AAA (4 u.) · Guante de cuero (2 u.) · Pack 2 guantes · MIX Bajo Par ·
Toalla + llavero · Hebilla · Lápiz.

No pierdas tiempo calculando márgenes de rodilleras y kinesio tape. **Empieza por
pelotas y guantes: son el 88% del ingreso.**

## Cómo usarla

### 1. Reunir los datos

Del producto (Shopify):
```
mcp__Shopify__get-product  → precio, variantes, tags
```
Tag `dropi` → el COGS es el costo Dropi. Tag `fulfillment-propio` → costo de compra propio.

⚠️ **El COGS no está en Shopify.** Pídelo al usuario si no lo tienes. **Nunca lo inventes** — un margen calculado sobre un COGS supuesto es peor que no calcular nada. Si el usuario no lo tiene a mano, entrega el cálculo en forma de tabla de sensibilidad (COGS al 30%, 40%, 50% del precio) y deja que él ubique la fila real.

### 2. CAC real

```sql
FROM sales SHOW orders GROUP BY order_referrer_source SINCE -30d UNTIL today
```
`CAC = gasto publicitario del período / órdenes atribuidas`. El gasto sale de MCP META. Si no hay atribución confiable, usa `gasto total / órdenes totales` y **dilo explícitamente**.

### 3. Umbrales de decisión

| Margen de contribución | Veredicto |
|---|---|
| <15% del precio lista | 🔴 No escalable. No pagar tráfico. Subir precio o cambiar producto. |
| 15–30% | 🟡 Solo orgánico o AOV alto vía bundle. |
| 30–50% | 🟢 Soporta tráfico pagado con disciplina. |
| >50% | 🟢 Producto ancla. Aquí va el presupuesto. |

### 4. Precio mínimo viable

```
P_min = (COGS + envío + CAC_objetivo) / (1 − 0,035 − margen_objetivo) × 1,19
```

Redondea **hacia arriba** al `.990` más cercano (convención chilena).

## Reglas duras

1. **Ningún cambio de precio sin este cálculo.** Sin excepción.
2. **Ningún descuento >15% sin recalcular.** Con 38,2% de fuga de descuento en la línea base, esto ya pasó una vez.
3. Un descuento del 100% (`net_sales = 0`) es un regalo. Debe ser una decisión consciente y registrada, no un accidente de configuración.
4. Envío: **lo paga el cliente (3.100)**. Entra en la cascada solo por la diferencia entre lo cobrado y el costo real de despacho. Si el courier cobra más de 3.100, esa diferencia sale del margen.
5. Si el margen sale negativo, **dilo en la primera línea**. No lo entierres bajo la tabla.
6. Al comparar productos, ordena por **margen absoluto en CLP**, no por porcentaje. Un 60% sobre 5.000 pierde contra un 30% sobre 35.000.
