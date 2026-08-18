---
name: ficha-producto
description: Escribe o reescribe fichas de producto de Albatro Golf en español de Chile, optimizadas para conversión — titular, bullets de beneficio, manejo de objeciones, tallas, envío y devoluciones, FAQ y JSON-LD. Úsala al crear un producto nuevo, al publicar un DRAFT, al mejorar una ficha que no convierte, o cuando la tasa de add-to-cart esté baja. Ataca directamente el paso más roto del embudo.
---

# Ficha de producto — Albatro Golf

**Por qué existe:** 1,07% de add-to-cart contra un benchmark de 6–10%. 98,9% de las visitas no agregan nada al carro. Quien agrega, sí llega al checkout. **La ficha es el cuello de botella.**

## Diagnóstico previo (obligatorio)

Lee la ficha actual antes de escribir. Marca cuál de estos falla:

- [ ] El titular dice **qué es** en vez de **qué resuelve**
- [ ] Beneficios clínicos ("compresión graduada") en vez de beneficios de juego ("estabilidad en el swing")
- [ ] Sin tabla de tallas → devoluciones y abandono
- [ ] Sin plazo de envío explícito **en días, a Chile**
- [ ] Sin política de devolución visible
- [ ] Foto de proveedor (CDN MercadoLibre `D_NQ_NP_*`) o generada por IA (`Gemini_Generated_Image_*`)
- [ ] Descripción cortada a media frase
- [ ] Sin manejo de la objeción real de compra

## Estructura

### 1. Titular — el problema, no el producto

El cliente es **golfista**, no paciente. Traduce siempre a lenguaje de juego.

| ❌ | ✅ |
|---|---|
| Rodillera de Compresión Ortopédica | Rodillera de compresión — estabilidad de rodilla los 18 hoyos |
| Kinesio Tape 5cm x 5m | Kinesio tape — soporte de codo y muñeca sin perder movilidad de swing |
| Faja Lumbar | Faja lumbar — protege la zona baja en cada rotación |

### 2. Primer párrafo — 2 frases

Frase 1: el momento de dolor concreto en la cancha.
Frase 2: qué cambia con el producto.

> *"Al hoyo 12 la rodilla empieza a avisar y el swing se acorta solo. Esta rodillera mantiene la articulación estable para que el último tercio de la ronda se juegue igual que el primero."*

Sin superlativos vacíos. Sin "revolucionario", "el mejor", "increíble".

### 3. Bullets — 4 a 6, formato beneficio → mecanismo

```
**Estabilidad en la rotación** — las bandas laterales limitan el desplazamiento
lateral de la rótula durante el giro de cadera.
```

Beneficio en negrita primero. El mecanismo justifica, no lidera.

### 4. Manejo de objeciones

La objeción real del golfista chileno, en orden de frecuencia:

1. **"¿Me va a estorbar el swing?"** → responder con movilidad, grosor, ajuste
2. **"¿Qué talla soy?"** → tabla de medidas **en cm**, con cómo medirse
3. **"¿Cuánto se demora?"** → días hábiles reales a Santiago y a regiones
4. **"¿Y si no me sirve?"** → política de cambio, explícita
5. **"¿Es de verdad para golf o es genérico?"** → el más importante en el catálogo Dropi. Si el producto es genérico, **el ángulo de golf tiene que estar en el uso, no en una mentira sobre el origen.**

### 5. Bloque de envío y devoluciones

Siempre visible en la ficha, no solo en una página aparte:

```
📦 Envío a todo Chile · Santiago 2–3 días hábiles · regiones 3–6 días hábiles
🔄 Cambio de talla sin costo dentro de 30 días
🧾 Boleta electrónica incluida
```

⚠️ Confirmar los plazos reales con el usuario antes de publicarlos. **Un plazo inventado es una promesa incumplida.**

### 6. FAQ — 3 a 5 preguntas

Preguntas reales de compra, no rellenas. Cada una en 1–2 frases.

### 7. JSON-LD

```json
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "…",
  "image": ["…"],
  "description": "…",
  "brand": { "@type": "Brand", "name": "Albatro Golf" },
  "offers": {
    "@type": "Offer",
    "priceCurrency": "CLP",
    "price": "9990",
    "availability": "https://schema.org/InStock",
    "url": "https://www.albatrogolf.cl/products/…"
  }
}
```

Sin `aggregateRating` mientras no haya reseñas reales. Inventarlas viola las políticas de Google y de la ley del consumidor chilena.

### 8. SEO on-page

- **Title** ≤60 car., keyword al inicio: `Rodillera de compresión para golf | Albatro Golf`
- **Meta description** ≤155 car., con beneficio y llamada a la acción
- **Handle** en minúsculas con guiones, sin stopwords
- **Alt text** descriptivo en cada imagen

## Antes de publicar

- [ ] Margen verificado con `/economia-unitaria`
- [ ] Tabla de tallas si aplica
- [ ] Plazos de envío **confirmados con el usuario**
- [ ] Foto propia, o marcada como pendiente de reemplazo
- [ ] SKU en formato `CAT-PRODUCTO-VARIANTE`
- [ ] `productType` en la taxonomía normalizada (no crear categorías nuevas)
- [ ] Ninguna afirmación médica de tratamiento o cura

## Reglas

1. **Nunca publiques un DRAFT como ACTIVE sin el checklist completo.**
2. Escritura en Shopify **solo con confirmación del usuario.**
3. Sin afirmaciones médicas. "Apoya", "ayuda a", "da soporte" — nunca "cura", "trata", "elimina el dolor". El SERNAC y el ISP chileno sancionan esto.
4. Español de Chile, tuteo. Nada de "vosotros" ni español neutro-España.
5. Si el producto es genérico de dropshipping, el ángulo de golf va en **el caso de uso**, no en una afirmación falsa de diseño o fabricación.
