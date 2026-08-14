---
name: auditoria-funnel
description: Autopsia completa del embudo de conversión y márgenes de Albatro Golf usando ShopifyQL. Úsala cuando el usuario pida revisar cómo va la tienda, por qué no vende, análisis de ventas, revisión semanal/mensual, "cómo vamos", diagnóstico de conversión, o antes de decidir dónde invertir. Detecta automáticamente fugas de descuento, productos con ingreso neto cero, tráfico que no convierte y pasos rotos del embudo.
---

# Auditoría de embudo — Albatro Golf

Diagnóstico en 6 pasos. **No saltes pasos**: el orden importa porque cada uno acota al siguiente.

## Paso 1 — Embudo

```sql
FROM sessions SHOW sessions, sessions_with_cart_additions, sessions_that_reached_checkout, sessions_that_completed_checkout, conversion_rate SINCE -30d UNTIL today
```

Calcula las tres tasas **entre pasos** (no sobre el total — ese es el error clásico):

| Tasa | Fórmula | Benchmark | Si falla, el problema es |
|---|---|---|---|
| Add-to-cart | `carro / sesiones` | 6–10% | Ficha de producto, precio, tráfico mal calificado |
| Checkout | `checkout / carro` | 60–80% | Costo de envío sorpresa, carro confuso |
| Cierre | `compró / checkout` | 45–65% | Medios de pago, impuesto añadido, fricción de formulario |

**Regla de diagnóstico:** la tasa más baja *relativa a su benchmark* es el único problema que importa esta semana. Arreglar cualquier otra cosa es desperdicio.

> Línea base 2026-08-14: ATC **1,07%** (6× bajo), checkout 77,6% (sano), cierre **28,9%** (2× bajo).
> El problema es la **ficha**, no el checkout.

## Paso 2 — Fuente de tráfico

```sql
FROM sessions SHOW sessions GROUP BY referrer_source ORDER BY sessions DESC SINCE -30d UNTIL today
FROM sales SHOW gross_sales, discounts, net_sales, taxes, total_sales, orders GROUP BY order_referrer_source ORDER BY total_sales DESC SINCE -30d UNTIL today
```

Cruza sesiones contra órdenes por fuente. Marca en rojo cualquier fuente con **>500 sesiones y <0,3% de conversión** — es gasto o esfuerzo tirado.

> Línea base: `social` 2.293 sesiones → **1 orden (0,04%)**. Si ese tráfico es pagado, cada peso ahí está financiando visitas que no compran.

## Paso 3 — Fuga de descuentos

```sql
FROM sales SHOW orders, gross_sales, net_sales, total_sales, average_order_value SINCE -30d UNTIL today
```

Calcula `fuga = (gross_sales − net_sales) / gross_sales`.

- <10% → normal
- 10–25% → revisar
- **>25% → alarma.** Es descuento descontrolado, devoluciones, o códigos al 100%.

> ⚠️ `sales_reversals` no existe como columna en esta tienda. Para separar descuento de devolución, usa `discounts` del Paso 2 y resta.

## Paso 4 — Productos con ingreso neto cero

```sql
FROM sales SHOW gross_sales, net_sales, orders GROUP BY product_title ORDER BY gross_sales DESC LIMIT 20 SINCE -30d UNTIL today
```

**Busca filas con `gross_sales > 0` y `net_sales = 0`.** Significa 100% descontado o 100% reembolsado.

Por cada una, pregunta al usuario: ¿regalo intencional, producto de prueba, o código de descuento fuera de control? No asumas.

> Línea base: 4 productos, 136.960 CLP en gross, **0 en neto**.

## Paso 5 — Concentración de ingreso

Del mismo query: ¿qué % del `net_sales` viene del top 3?

Contrasta contra el **peso en catálogo**. Si el 80% del catálogo genera <10% del ingreso, hay desalineación entre dónde va el esfuerzo y dónde está el dinero.

> Línea base: el ingreso real viene del **golf core** (pelotas usadas, guantes, toallas). El catálogo Dropi de recovery/salud ocupa la mayoría de las fichas y casi no aparece en el top 10.

## Paso 6 — Informe

Entrega **exactamente** esta estructura. Sin relleno.

```markdown
## Auditoría Albatro Golf — [fecha] (ventana [N]d)

### El número
[La métrica más rota, con su benchmark y el múltiplo de distancia.]

### Embudo
| Paso | Valor | Tasa | Benchmark | Estado |

### Dónde se va la plata
[Fuga de descuentos %, productos en neto cero, tráfico que no convierte.]

### Las 3 acciones de esta semana
1. [Acción] → mueve [métrica] → esfuerzo [alto/medio/bajo]
2. …
3. …

### Lo que NO hay que hacer todavía
[Lo que parece urgente y no lo es, con la razón.]
```

## Reglas

- **Solo lectura.** Esta skill nunca escribe en Shopify.
- Si un query falla, reporta el error textual — no inventes el dato.
- Con <30 órdenes en la ventana, di explícitamente que la muestra es chica y que las tasas tienen ruido alto. No presentes 11 órdenes como tendencia estadística.
- Compara siempre contra la línea base de `CLAUDE.md` y **actualízala** si cambió materialmente.
- Máximo 3 acciones. Una lista de 10 no se ejecuta.
