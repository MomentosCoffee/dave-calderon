# 📸 Prompts para variaciones fotográficas de Dave Calderón

> Guía para generar imágenes coherentes a partir de la foto base de Dave (smoking + NYC skyline). Para usar en Midjourney v6/v7, Gemini Imagen 3, ChatGPT Image, o Sora (video).

---

## 🎯 Sistema visual a mantener (consistencia)

Para que TODAS las imágenes se vean de la misma marca, mantener estos parámetros estables:

**Color grading:** Cinematic. Blacks profundos (charcoal, no negro puro), highlights cálidos (skin tone naranja-dorado), midtones ligeramente desaturados. Estilo Roger Deakins / "Sicario" / GQ editorial.

**Iluminación:** Lateral o backlight. Nunca flash duro frontal. Sombras presentes pero suaves. Hora azul / golden hour preferidas.

**Composición:** Dave en el tercio derecho del frame típicamente, mirando a cámara o ligeramente de lado. Espacio negativo a la izquierda para overlay de texto. Wide enough para mostrar contexto.

**Vestuario:** Traje formal oscuro (charcoal, navy oscuro, negro). Variaciones aceptables: blazer + camisa + sin corbata, traje con corbata, esmoquin (foto base). Evitar logos, colores estridentes, ropa casual deportiva.

**Expresión:** Seria-amable. NO sonrisa amplia tipo retrato corporativo americano. Más bien la mirada calma y presente del personaje "alguien en quien confiar en una crisis".

**Aspect ratio:** 16:9 ultra-wide para hero/secciones. 4:5 portrait para "Sobre Dave".

---

## 🛠️ Mejor herramienta para coherencia con Dave

**Recomendada: Midjourney v6/v7 con `--cref`**

```
[prompt] --cref [URL_pública_foto_de_Dave] --cw 80 --style raw --ar 16:9 --v 7
```

`--cref` mantiene los rasgos faciales de Dave consistentes. `--cw 80` da peso fuerte al referente. Subir la foto de Dave a Imgur o Cloudinary primero para tener URL pública.

**Alternativa: Gemini Imagen 3 / Google Whisk**

Subir foto de Dave como referencia, usar prompt sin `--cref` (Imagen 3 acepta image-to-image directamente).

**Para video del hero (5-10 seg):** Sora o Runway Gen-3 con la imagen generada como starting frame.

---

## 🖼️ Las 7 imágenes que necesitas

### 1. HERO — NYC skyline noche (panorámica)
> **Reemplaza:** `https://picsum.photos/seed/dave-hero-ny/2400/1600`

```
Cinematic editorial portrait of Dave Calderón, late twenties Latino man with dark wavy hair, wearing a charcoal tailored suit, standing at the foreground right of the frame. Background: New York City skyline at twilight transitioning into blue hour, Brooklyn Bridge silhouetted in the mid-distance, soft city lights creating warm bokeh. Shot on Hasselblad medium format, 50mm equivalent, shallow depth of field. Dave in sharp focus, expression confident and approachable but not smiling broadly. Color grading: warm golden highlights, charcoal blacks, slightly desaturated midtones. Roger Deakins cinematic style. NO text, NO logos. Aspect ratio 16:9 ultrawide
```

---

### 2. AUTO ACCIDENT — Highway/calle urbana noche
> **Reemplaza:** `https://picsum.photos/seed/dave-auto/2400/1400`

```
Cinematic editorial portrait of Dave Calderón [same person, same wardrobe direction: dark suit], standing at the side of a wet city street at night. Background: blurred red and white car lights streaking from passing traffic, slight rain reflecting on asphalt, distant neon signs creating warm glow. Shallow depth of field, Dave in sharp focus right third of frame, traffic blurred in motion. Mood: serious, present, watchful. Color grading: deep blacks, red and amber accents from city lights, cinematic Roger Deakins style. NO car crash visible, NO blood, NO emergency vehicles. NO text. Aspect ratio 16:9
```

---

### 3. WORK INJURY — Sitio de construcción atardecer
> **Reemplaza:** `https://picsum.photos/seed/dave-work/2400/1400`

```
Cinematic editorial portrait of Dave Calderón, dark blazer over a dark crewneck, standing in front of a large urban construction site at golden hour. Background: silhouettes of construction cranes against orange-pink sky, scaffolding structures, partially built skyscraper. Dave in mid-distance right of frame, expression contemplative and grounded. Industrial yet elegant. Warm rim light on Dave's profile. NO workers visible in dangerous positions, NO blood, NO accidents shown. NO text, NO logos on equipment. Aspect ratio 16:9, cinematic color grading with warm highlights and deep shadows
```

---

### 4. SLIP & FALL — Edificio comercial interior nocturno
> **Reemplaza:** `https://picsum.photos/seed/dave-fall/2400/1400`

```
Cinematic editorial portrait of Dave Calderón, dark suit, standing in the foreground right of a vast empty modern shopping mall corridor at night. Background: polished marble floors with dramatic reflections, vacant escalators visible in the distance, ambient cool blue lighting mixed with warm spotlights from above. Architectural lines, vast empty space conveying isolation and neglect. Dave looking at camera, calm presence. NO accident visible, NO yellow caution signs, NO injured people. NO text. Aspect ratio 16:9, moody cinematic, Wong Kar-wai meets Edward Hopper
```

---

### 5. DOG BITE — Calle residencial atardecer
> **Reemplaza:** `https://picsum.photos/seed/dave-dog/2400/1400`

```
Cinematic editorial portrait of Dave Calderón, dark blazer with dark t-shirt underneath, standing in a quiet residential American suburban street at twilight. Background: row of suburban houses with porch lights warming on, autumn leaves on the sidewalk, streetlamp casting amber glow. Dave standing thoughtfully right of frame. NO dogs visible, NO injuries, NO blood. Mood: pensive, conveying that injuries can happen anywhere even in calm places. Cinematic color grading, soft blue-amber palette. NO text. Aspect ratio 16:9
```

---

### 6. CONTACT — Oficina/escritorio noche (24/7 vibe)
> **Reemplaza:** `https://picsum.photos/seed/dave-contact/2400/1600`

```
Cinematic editorial portrait of Dave Calderón seated at a dark wooden desk in a moody home office at night, only a small green-shaded desk lamp lighting the scene. Background: dark wooden bookshelf with legal-looking books out of focus, golden warm lamp glow creating Rembrandt lighting on Dave's face. Dave holding a phone to his ear, expression listening intently and calm. Conveying: "24/7 available, talking to someone right now." Dark suit with rolled sleeves. NO text, NO logos. Aspect ratio 16:9, cinematic film noir aesthetic
```

---

### 7. ABOUT — Retrato cerrado estudio
> **Reemplaza:** `images/dave-hero.jpg` (cuando consigas mejor foto)

```
Cinematic editorial close-up portrait of Dave Calderón, head and shoulders, dark charcoal suit and crisp white shirt without tie. Studio background: dark moody gradient (charcoal to deep grey), single key light from upper left creating Rembrandt lighting (small triangle of light on the cheek opposite the light source). Looking directly at camera, calm confident expression with the slightest hint of warmth. Style: GQ magazine cover, Annie Leibovitz lighting, sharp eyes, beautiful skin texture detail. Aspect ratio 4:5 portrait
```

---

## 🎬 Bonus: Video hero (5-10 segundos)

Si quieres reemplazar el hero estático con video sutil:

```
5 second cinematic clip: Dave Calderón standing on a Manhattan rooftop at twilight with NYC skyline behind him. Camera slowly pushes in on him from medium shot to medium close-up. Dave looking off-frame to the right then turning his gaze toward camera in the final 2 seconds. Ambient city sounds. No dialogue. Slight wind moving his hair and jacket. Color grade: cinematic, warm highlights, charcoal blacks. 16:9, 24fps, shot on ARRI Alexa style
```

Subir como `images/hero-bg.mp4` y reemplazar el `<img>` del hero por:
```html
<video autoplay muted loop playsinline class="w-full h-full object-cover">
  <source src="images/hero-bg.mp4" type="video/mp4">
</video>
```

---

## 📋 Checklist al recibir las imágenes

Antes de subirlas a `/images/`:

- [ ] Comprimir con `sips` (macOS): `sips -Z 2000 -s format jpeg --setProperty formatOptions 80 input.jpg --out output.jpg`
- [ ] Versión OG landscape 1200×630: recortar el hero a esa proporción
- [ ] Verificar que Dave es reconocible y consistente en todas
- [ ] No hay texto/logos/marcas accidentales
- [ ] No hay elementos legalmente sensibles (sangre, sufrimiento explícito, vehículos accidentados con detalles)

## 🔁 Reemplazos en `index.html`

Una vez tengas las imágenes en `/images/`, reemplaza:

| Dónde | URL Picsum actual | Cambiar por |
|---|---|---|
| Hero | `picsum.photos/seed/dave-hero-ny/2400/1600` | `images/hero.jpg` |
| Auto | `picsum.photos/seed/dave-auto/2400/1400` | `images/auto.jpg` |
| Work | `picsum.photos/seed/dave-work/2400/1400` | `images/work.jpg` |
| Fall | `picsum.photos/seed/dave-fall/2400/1400` | `images/fall.jpg` |
| Dog | `picsum.photos/seed/dave-dog/2400/1400` | `images/dog.jpg` |
| Contact | `picsum.photos/seed/dave-contact/2400/1600` | `images/contact.jpg` |
| About | `images/dave-hero.jpg` | `images/about.jpg` |

Find&replace global en el HTML con tu editor.
