---
name: seo-chile
description: Plan de keywords y contenido para búsqueda orgánica en Chile para Albatro Golf — investigación de términos en español chileno, arquitectura de colecciones, artículos de blog y schema local. Úsala cuando el usuario pregunte por SEO, tráfico orgánico, posicionamiento en Google, blog, o cómo dejar de depender de la publicidad pagada.
---

# SEO Chile — Albatro Golf

**Por qué existe:** 53 sesiones de búsqueda en 90 días, sobre 4.576 totales. **1,2%.** Una tienda de nicho sana saca 20–40% del orgánico. Es el canal más barato y hoy está en cero.

**Por qué es ganable:** el golf en Chile es un nicho chico con poca competencia de SEO en español. Los términos long-tail son alcanzables sin autoridad de dominio alta.

## Paso 1 — Mapa de intención

Tres capas, en este orden de prioridad:

### Transaccional (dinero, primero)
Producto + calificador local. Baja competencia, alta intención.
```
guantes de golf chile
pelotas de golf usadas chile
comprar accesorios de golf santiago
rodillera para golf
guante de cuero para golf precio
```

### Comparativa (mitad del embudo)
```
mejores guantes de golf para clima húmedo
pelotas grado AAA vs AAAA diferencia
qué guante de golf comprar según la mano
```

### Informativa (arriba del embudo, construye autoridad)
```
cómo evitar el codo de golfista
ejercicios de movilidad para el swing
cómo lavar un guante de golf de cuero
qué llevar en la bolsa de golf
canchas de golf públicas en chile
```

⚠️ **Verifica volumen antes de comprometer esfuerzo.** Usa WebSearch para confirmar que los términos se usan realmente en Chile y ver quién rankea. No inventes cifras de volumen de búsqueda — si no tienes acceso a una herramienta de keywords, dilo y trabaja con intención cualitativa.

## Paso 2 — Arquitectura

```
/                              → marca + oferta principal
/collections/guantes           → transaccional principal
/collections/pelotas           → transaccional principal
/collections/accesorios
/collections/recovery          → catálogo Dropi, agrupado
/products/[handle]             → una keyword transaccional por ficha
/blogs/consejos/[handle]       → informativo, enlaza a colección
```

**Regla:** cada artículo del blog enlaza a **una** colección o ficha relevante con anchor descriptivo. Sin enlace comercial, un artículo es decoración.

## Paso 3 — Prioridad de contenido

Ordena por `(intención comercial × alcanzabilidad) / esfuerzo`. Primero lo transaccional: una colección optimizada rinde más que diez artículos.

| Prioridad | Acción | Esfuerzo |
|---|---|---|
| 1 | Optimizar colecciones existentes (title, meta, 150–300 palabras de intro) | Bajo |
| 2 | Fichas de los productos que ya venden (pelotas, guantes) | Bajo |
| 3 | 3–5 artículos informativos que enlacen a esas colecciones | Medio |
| 4 | Página de marca / historia (búsquedas de marca) | Bajo |

## Paso 4 — Técnico

- [ ] `Organization` + `LocalBusiness` schema con dirección chilena
- [ ] `BreadcrumbList` en fichas y colecciones
- [ ] `hreflang="es-CL"`
- [ ] Sitemap enviado a Google Search Console — **verificar que existe la propiedad**
- [ ] Core Web Vitals: el peso de imagen es el asesino habitual en Shopify. Comprimir y servir WebP.
- [ ] Sin contenido duplicado entre variantes

## Paso 5 — Medición

```sql
FROM sessions SHOW sessions GROUP BY referrer_source ORDER BY sessions DESC SINCE -30d UNTIL today
```

Objetivo: `search` de **53 a 400+ sesiones/90d** en dos trimestres. Revisar mensualmente.

Google Search Console da los datos de verdad (impresiones, posición, CTR). Shopify solo cuenta sesiones. **Si no está configurado, ese es el paso cero.**

## Reglas

1. Español de Chile. "Cancha" no "campo". "Palos" no "bastones". "Auto" no "coche".
2. Sin keyword stuffing. Google en español lo penaliza igual.
3. Cada pieza de contenido tiene una keyword objetivo y un destino comercial. Si no, no se escribe.
4. Sin afirmaciones médicas en contenido de recovery (ver `/ficha-producto`).
5. SEO rinde en 3–6 meses. **Dilo explícitamente** al presentar el plan — no lo vendas como solución al problema de conversión de esta semana.
