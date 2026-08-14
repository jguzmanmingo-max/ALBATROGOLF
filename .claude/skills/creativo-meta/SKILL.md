---
name: creativo-meta
description: Ciclo cerrado de creatividad publicitaria para Albatro Golf — analiza qué anuncios funcionan con Motion Creative Analytics, redacta el brief, produce el asset con Higgsfield o Canva y lo sube a Meta. Úsala cuando el usuario quiera hacer anuncios, mejorar creatividades, entender por qué la publicidad no convierte, o producir video y ferias de imágenes para campañas.
---

# Ciclo creativo Meta — Albatro Golf

**Por qué existe:** el stack ya está conectado — Motion Creative Analytics, MCP META, Higgsfield, Canva, Adobe. Lo que falta es el **circuito** que los une. Hoy son cinco herramientas sueltas.

## ⚠️ Puerta de entrada — leer antes de producir nada

Línea base: **2.293 sesiones de `social` → 1 orden. 0,04% de conversión.**

Con ese número, **más creatividades no arreglan nada.** El tráfico ya llega; se pierde en la ficha (add-to-cart 1,07%).

**Antes de gastar un peso más en producción creativa:**

1. Corre `/auditoria-funnel`. Confirma si el problema sigue siendo la ficha.
2. Corre `/economia-unitaria`. Calcula el **CAC máximo pagable** al AOV actual (18.890 CLP).
3. Si el CAC máximo es menor al CPA real de Meta, **la respuesta correcta es pausar, no producir.**

Dile esto al usuario de frente. Producir creatividades para un embudo roto es quemar plata con mejor arte.

## Paso 1 — Qué está funcionando

```
mcp__Motion_Creative_Analytics__get_auth_context     → workspaceId
mcp__Motion_Creative_Analytics__get_creative_insights → insightType=SPEND primero (regla del MCP)
mcp__Motion_Creative_Analytics__get_creative_insights → luego SCALING, luego HOOK
```

**Regla del servidor Motion: `SPEND` siempre primero** — trae `goalMetric` y `spendThreshold`, necesarios para interpretar el resto.

Extrae:
- **Hook rate** — ¿retiene los primeros 3 segundos?
- **Hold rate** — ¿llega al final?
- Qué formato, ángulo y primer frame comparten los ganadores

Para competencia y referencias:
```
mcp__Motion_Creative_Analytics__get_inspo_creatives
mcp__Motion_Creative_Analytics__get_workspace_competitors
```

## Paso 2 — Brief

Una plantilla por concepto. Sin esto, la producción es decorativa.

```markdown
### Concepto: [nombre]
**Producto:** [con margen verificado]
**Audiencia:** golfista chileno, [segmento]
**Dolor:** [momento concreto en la cancha]
**Hook (0–3s):** [qué se ve y se oye exactamente]
**Desarrollo (3–15s):** [demostración]
**CTA:** [acción + destino]
**Formato:** 9:16 · [duración]
**Objeción que resuelve:** [una]
```

**El hook es el 80% del resultado.** Escríbelo como plano concreto, no como idea abstracta.

## Paso 3 — Producir

| Necesidad | Herramienta |
|---|---|
| Video con actor / UGC | `higgsfiels__get_workflow_instructions` → workflow UGC, luego `generate_video` |
| Video de producto | `higgsfiels__generate_video` con imagen de referencia |
| Voz en off en español | `higgsfiels__generate_audio` |
| Estático / carrusel | `Canva__generate-design` o `Adobe__create_visual_design_express_skill` |
| Foto de producto limpia | `Adobe__image_remove_background` + `image_apply_adjustments` |
| Adaptar a otro formato | `higgsfiels__reframe` |

**Para video de marca serio, pasa por `get_workflow_instructions` primero** — trae el protocolo completo. No improvises el flujo.

Predicción de rendimiento antes de publicar:
```
mcp__higgsfiels__virality_predictor
```

## Paso 4 — Publicar

```
mcp__MCP_META__ads_creative_upload_video / upload_image
```

⚠️ **Toda escritura en Meta (subir creatividad, cambiar presupuesto, lanzar campaña) requiere confirmación explícita del usuario.** Esto gasta dinero real.

## Paso 5 — Medir

Espera **mínimo 3–5 días o 50 clics** antes de juzgar. Antes de eso es ruido.

Vuelve al Paso 1. Un ganador es punto de partida para 3 variaciones, no un final.

## Reglas

1. **Nunca produzcas sin pasar la puerta de entrada.** El embudo primero.
2. Nada de afirmaciones médicas en el copy publicitario — Meta rechaza y el SERNAC sanciona.
3. Español de Chile. Un acento neutro-latino genérico rinde peor que el local.
4. Producto con margen <15% no va a tráfico pagado. Sin excepciones (ver `/economia-unitaria`).
5. Nunca uses fotos de proveedor o de MercadoLibre en anuncios — riesgo legal y se ve genérico.
6. Una variable por test. Cambiar hook y oferta a la vez no enseña nada.
