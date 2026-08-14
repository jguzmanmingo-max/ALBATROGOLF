# Capa de agente — ALBATROGOLF

Esta carpeta convierte a Claude de asistente genérico en **operador de Albatro Golf**.

## Cómo funciona

- **`/CLAUDE.md`** (raíz del repo) se carga automáticamente en **toda** sesión. Contiene el contexto del negocio, los números actuales y las reglas duras. No hay que invocarlo.
- **`.claude/skills/*/SKILL.md`** son procedimientos que se activan solos cuando la conversación los amerita, o a mano con `/nombre-skill`.

## Skills

| Skill | Ataca | Invocación |
|---|---|---|
| `auditoria-funnel` | Diagnóstico: dónde se rompe el embudo y dónde se va la plata | `/auditoria-funnel` |
| `economia-unitaria` | Margen real. Precio mínimo viable. CAC máximo pagable. | `/economia-unitaria` |
| `ficha-producto` | Add-to-cart 1,07% → el cuello de botella #1 | `/ficha-producto` |
| `seo-chile` | 53 sesiones orgánicas en 90 días | `/seo-chile` |
| `creativo-meta` | Social: 2.293 sesiones → 1 orden | `/creativo-meta` |

## Orden recomendado

```
1. /auditoria-funnel      ← empieza siempre acá. Sin diagnóstico no hay prioridad.
2. /economia-unitaria     ← antes de tocar precios o pagar tráfico
3. /ficha-producto        ← el arreglo de mayor impacto hoy
4. /seo-chile             ← el canal barato, rinde en 3–6 meses
5. /creativo-meta         ← último. Solo con el embudo ya arreglado.
```

## Mantenimiento

- Cuando los números cambien materialmente, **actualiza la línea base en `CLAUDE.md`**. Una línea base vieja es peor que ninguna: hace que el agente compare contra una realidad que ya no existe.
- Al agregar una skill, agrégala también a la tabla de `CLAUDE.md` §6.
- Las consultas ShopifyQL de `CLAUDE.md` §7 están **verificadas contra esta tienda**. Si agregas una, verifícala antes de dejarla escrita.

## Regla transversal

**Lectura libre, escritura con confirmación.** Ninguna skill modifica precios, estados de producto, inventario, descuentos ni campañas de Meta sin que el usuario lo apruebe explícitamente. Todo eso mueve dinero real.
