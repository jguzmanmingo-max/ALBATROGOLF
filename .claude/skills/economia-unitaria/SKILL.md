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

### Ejemplo trabajado — Rodillera de Compresión Ortopédica

| Concepto | CLP |
|---|---|
| Precio lista (IVA incl.) | 9.990 |
| Neto de IVA (`9.990 / 1,19`) | 8.395 |
| COGS Dropi *(⚠️ pedir dato real)* | ~4.000 |
| Comisión pasarela (3,5% de 9.990) | 350 |
| Envío subsidiado *(línea base: 1.735/orden)* | 1.735 |
| **Margen de contribución** | **≈ 2.310** |
| Margen sobre precio lista | **23%** |

**Lectura:** con ~2.310 CLP por unidad, el CAC máximo pagable es 2.310 — y eso deja **cero** utilidad. Para 15% de utilidad real el CAC no puede pasar de ~800 CLP (~USD 0,85). **En Meta, a ese ticket, es prácticamente inalcanzable.**

→ Conclusión de negocio: los productos de 9.990 **no soportan tráfico pagado**. Solo funcionan como orgánico, como bundle, o como cross-sell dentro de una orden ya adquirida.

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
4. Envío: hoy se subsidia ~1.735 CLP por orden. Ese número entra **siempre** en la cascada.
5. Si el margen sale negativo, **dilo en la primera línea**. No lo entierres bajo la tabla.
6. Al comparar productos, ordena por **margen absoluto en CLP**, no por porcentaje. Un 60% sobre 5.000 pierde contra un 30% sobre 35.000.
