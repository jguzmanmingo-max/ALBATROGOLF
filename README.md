# ALBATROGOLF

Tienda de golf en Chile — [albatrogolf.cl](https://www.albatrogolf.cl)

Este repositorio es la **capa de agente** de la operación: el contexto de negocio y los
procedimientos que Claude Code usa para trabajar sobre la tienda Shopify, Meta Ads y la
producción de contenido.

## Estructura

```
CLAUDE.md                 Contexto del negocio + números + reglas. Se carga en toda sesión.
.claude/
  README.md               Cómo usar esta capa
  skills/
    auditoria-funnel/     Diagnóstico de embudo y márgenes
    economia-unitaria/    Margen real, precio mínimo, CAC máximo
    ficha-producto/       Fichas optimizadas a conversión
    seo-chile/            Búsqueda orgánica en Chile
    creativo-meta/        Ciclo Motion → Higgsfield/Canva → Meta
index.html                Stub sin uso — pendiente de decisión
```

## Empezar

```
/auditoria-funnel
```

Diagnostica antes de actuar. El resto de las skills dependen de saber qué está roto.

## Estado (corte 2026-08-14, ventana 90 días)

| Métrica | Valor | Benchmark |
|---|---|---|
| Conversión | 0,24% | 1,4–2% |
| Add-to-cart | 1,07% | 6–10% |
| Fuga de descuentos | 38,2% | <10% |
| Tráfico orgánico | 1,2% | 20–40% |

Detalle completo y lectura en [`CLAUDE.md`](./CLAUDE.md).
