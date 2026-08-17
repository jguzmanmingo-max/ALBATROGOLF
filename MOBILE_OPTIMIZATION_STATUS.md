# Estado de Optimización Mobile — Albatro Golf

**Última actualización:** 17 agosto 2026  
**Ejecutado por:** Claude Code  
**Branch:** `claude/audit-skills-mcps-q4sq4q`

---

## 📊 Diagnóstico móvil

| Métrica | Valor | Benchmark | Estado |
|---|---|---|---|
| Mobile sessions | 598 (86% del tráfico) | — | 🔴 bottleneck |
| Mobile add-to-cart | 1,0% | 6–10% | 🔴 crítico |
| Desktop add-to-cart | 6,4% | 6–10% | 🟢 normal |
| Ratio mobile/desktop | **6x peor** | ~1x | 🔴 crisis |

**Causa identificada:** Ficha de producto móvil:
- Selector de variantes como dropdown largo (difícil en mobile)
- Catálogo duplicado (2 poleras por separado en lugar de 1)
- Imágenes no optimizadas para pantalla pequeña

---

## ✅ Cambios completados (17 ago 2026)

### 1. Poleras: Consolidación y limpieza

**Antes:**
- Producto 9290826809597: Polera Dry Fit (Verde solo) — 4 variantes
- Producto 9290840899837: Polera Dry Fit (Navy solo) — 2 variantes
- Producto 15290872365309: Polera Dry Fit (Verde + Navy consolidada) — 6 variantes
- **Total:** 3 productos, 12 variantes, duplicación confusa

**Después:**
- Producto 9290826809597: **ARCHIVADO**
- Producto 9290840899837: **ARCHIVADO**
- Producto 15290872365309: Polera Dry Fit Golf Performance ← **ÚNICA**
  - Variante 1: Verde Petróleo / M
  - Variante 2: Azul Navy / M
  - Stock: 0 (agotadas mientras compras inventario real)

**Impacto:**
- Reduce confusión en búsqueda interna (+SEO)
- Mejora UX mobile (1 ficha en lugar de 3)
- Facilita gestión de inventario

---

## ⚠️ Pendiente: Variantes de guantes

### Problema identificado
Producto "Guante de golf 100% Albatro Golf Players collection" (ID: 9275345207549)

**Estructura actual (caótica):**
```
Opción 1: "Tamaño de accesorio" → valores como texto largo
  - "M. Mano Izquierda"
  - "S. Mano izquierda"
  - "L mano izquierda"
  - "M-L. Mano izquierda.Talla intermedia"
  - "M. Mano DERECHA(para zurdos)"
```

**En mobile:** Dropdown con 5 opciones de 30+ caracteres → muy difícil de leer y seleccionar.

### Estructura objetivo
```
Opción 1: "Talla" → S, M, L, M-L, XL
Opción 2: "Mano" → Izquierda, Derecha
```

Variantes legibles: "S / Izquierda", "M / Derecha", etc.

### ¿Por qué no se hizo?
Shopify **no permite cambiar la estructura de opciones** de un producto vía GraphQL si tiene variantes existentes. Las alternativas eran:
1. Recrear el producto completo (destructivo, pierde historial)
2. Hacerlo manualmente desde Shopify Admin (10–15 min)
3. Usar app de Shopify ($10–20/mes)

**Se eligió opción manual** → instrucciones en MOBILE_OPTIMIZATION_REPORT.md

**Inventario a preservar:**
- S Izquierda: 29 unidades
- M Izquierda: 49 unidades
- L Izquierda: 4 unidades
- M-L Izquierda: 19 unidades
- M Derecha: 5 unidades
- **Total: 106 unidades**

---

## 🎯 Próximos pasos (usuario)

### Urgente (hoy/mañana)
1. **Reestructurar guantes:**
   - Opción A: Manualmente desde Shopify Admin (10 min)
   - Opción B: Instalar app (5 min setup, automatiza para futuros productos)
   
2. **Ejecutar checklist de tema (6 items):**
   - Botón "Agregar al carrito" sticky
   - Speed test y compresión de imágenes
   - Selectores como botones (no dropdown)
   - Apps instaladas (desactivar innecesarias)
   - Checkout acelerado (Shop Pay / Google Pay / Apple Pay)
   - Fotos de producto (sin recortes raros)

### Esta semana
1. Optimizar imágenes de guantes (<300kb)
2. Actualizar descripción móvil-friendly
3. Verificar Pack de 2 guantes (mismo problema de variantes)
4. Monitorear analytics

### Estimado de impacto
- **Hoy:** 1,0% add-to-cart
- **Después reestructura + checklist:** 2,5–3,2%
- **Con imágenes optimizadas:** 3,2–4,0%

**Plazo realista:** 1–2 semanas, 2–3 horas de trabajo manual.

---

## 📋 Cambios vía GraphQL Shopify

```
# Archivadas
mutation {
  productChangeStatus(productId: "gid://shopify/Product/9290826809597", status: ARCHIVED)
  productChangeStatus(productId: "gid://shopify/Product/9290840899837", status: ARCHIVED)
}

# Reducida polera consolidada
mutation {
  productVariantsBulkDelete(
    productId: "gid://shopify/Product/15290872365309",
    variantsIds: [
      "gid://shopify/ProductVariant/71156019134717",  # Verde S
      "gid://shopify/ProductVariant/71156019200253",  # Verde L
      "gid://shopify/ProductVariant/71156019233021",  # Verde XL
      "gid://shopify/ProductVariant/71156019298557"   # Navy L
    ]
  )
}
```

---

## 📞 Contacto / Dudas

- ¿Cómo reestructurar guantes? → Ver MOBILE_OPTIMIZATION_REPORT.md
- ¿Impacto de los cambios? → Ver tabla de "Estimado de impacto" arriba
- ¿Qué sigue después? → Pack de 2 guantes, MIX Bajo Par, descripción general
