# Contenido Albo — canal de tips y ejercicios de golf

Carpeta de trabajo para el canal de contenido (Shorts + YouTube) respaldado por **Albo**,
la mascota de marca de Albatro Golf. Extraído y reorganizado del correo
*"Albatro Golf — Avatar Albo + estrategia de contenido"* (18 ago 2026) y de la
estrategia original en PR #2 (rama `claude/golf-avatar-strategy-qaeiwj`).

**Concepto en una frase:** tú eres el protagonista (el golfista real, cara y voz).
Albo es la mascota que firma tus videos, aparece en miniaturas y conecta cada tip con
la tienda — no reemplaza tu cámara, la potencia.

## Cómo está organizada esta carpeta

```
contenido-albo/
├── estrategia/          → el documento completo (referencia, no se edita seguido)
├── marca/                → logos SVG + qué falta bajar (2 ilustraciones de Albo)
├── produccion/           → lo que se usa CADA SEMANA para grabar
│   ├── calendario.md          — qué toca grabar, en qué orden, casillero para marcar avance
│   ├── plantilla-*.md          — las 3 plantillas de guion, con checklist de grabar y publicar
│   └── episodios/               — un archivo por video ya escrito, listo para grabar
└── youtube/              → seguimiento de lo publicado
    └── tracker-publicacion.md
```

## Flujo de trabajo semanal

1. Abre `produccion/calendario.md`, mira qué toca esta semana.
2. Copia el bloque de guion vacío de la plantilla correspondiente (`plantilla-tip-rapido.md`,
   `plantilla-ejercicio.md` o `plantilla-producto.md`) en un archivo nuevo dentro de
   `produccion/episodios/` — usa `2026-W1-lun-error-swing.md` como ejemplo de cómo se ve
   uno ya completado.
3. Graba (guía de setup en `estrategia/estrategia-avatar-golf.md`, sección 5) en tandas:
   un día al mes, 2–3 horas, 8–12 videos de una vez.
4. Edita, sube, y marca la fila correspondiente en `youtube/tracker-publicacion.md`.
5. Marca el estado en `produccion/calendario.md` como hecho.

## Pendiente antes de la primera grabación

- [ ] **Bajar las 2 ilustraciones de Albo de cuerpo entero** — están en tu Canva o por
      link directo, ver `marca/personajes-pendientes.md`. Elegir una como oficial.
- [ ] Definir código de descuento para seguidores (la estrategia sugiere `ALBO10` como
      ejemplo — confirmar el código real antes de mencionarlo en video).
- [ ] Conseguir/confirmar micrófono de solapa y trípode o apoyo para el celular.

## Assets de marca — referencia rápida

| Pieza | Archivo | Úsala para |
|---|---|---|
| Logomark con fondo | `marca/albo-logomark.svg` | Foto de perfil, favicon, marca de agua, sello en miniaturas |
| Logomark sin fondo | `marca/albo-logomark-mono.svg` | Merch, sellos sobre fotos claras |
| Personaje cuerpo entero | *pendiente bajar* — ver `marca/personajes-pendientes.md` | Stickers de reacción, intro/outro, miniaturas |

**Paleta:** verde fairway `#1E5B41` (primario) · dorado albatros `#B8892B` (acento, con
moderación) · crema pergamino `#F6F5EC` (fondos claros) · verde noche `#14251D` (texto).

El documento completo — pilares de contenido, guía de personalidad de Albo, reglas de
uso del logo, bio/hashtags sugeridos y el plan B de avatar 100% IA — está en
`estrategia/estrategia-avatar-golf.md` (también hay una versión visual en
`estrategia/estrategia-albo.html`, abrir en el navegador).
