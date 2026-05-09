# Dave Calderón — Landing V0.1

Sitio estático para Dave Calderón (conector post-accidente, NO abogado), construido siguiendo el blueprint de Cordent (`../dr-manuel-corredor/docs/BLUEPRINT-METODOLOGIA.md`).

## 🚀 Stack

- HTML/CSS/JS puro, archivo único `index.html`
- Sin Node, sin build, sin dependencias
- Hosting: Vercel (auto-deploy desde GitHub)
- Forms: Formspree (placeholder hasta tener cuenta)
- Bilingüe ES/EN con toggle (default ES, persiste en localStorage)
- Schema.org `ProfessionalService` + `Person` + `FAQPage`

## 📁 Estructura

```
dave-calderon/
├── index.html          ← landing principal (todo CSS+JS inline)
├── gracias.html        ← post-submit
├── vercel.json
├── robots.txt
├── .gitignore
├── README.md
└── images/
    ├── dave-hero.jpg       ← FALTA: foto del esmoquin con NYC (ver TODOs)
    ├── dave-about.jpg      ← FALTA: foto secundaria (opcional)
    └── og-image.jpg        ← FALTA: 1200x630 para previews social
```

## ✅ TODOs antes de pautar

### Críticos (bloquean lanzamiento)

- [ ] **Guardar la foto del esmoquin como `images/dave-hero.jpg`** (Mateo la pasó por chat, hay que descargarla manualmente desde el chat al directorio)
- [ ] **Crear `images/og-image.jpg`** (1200×630, landscape, para previews en WhatsApp/FB). Recortar versión horizontal del hero photo
- [ ] **Confirmar con Dave que el número 516-233-7514 sigue siendo válido** (asumido de los flyers)
- [ ] **Crear cuenta Formspree** (https://formspree.io) → reemplazar `REPLACE_WITH_FORMSPREE_ID` en index.html línea ~735 con el ID real
- [ ] **Subir a GitHub** (repo nuevo) + conectar Vercel para auto-deploy
- [ ] **Comprar dominio** (sugerencia: `davecalderon.com` o `daveayuda.com`) y conectar a Vercel
- [ ] **Validar copy con Dave** (especialmente disclaimers) y, si es posible, con un abogado de NY/USA antes de pautar — hay leyes anti-runner/capper que aplican

### Importantes (mejoran conversión y SEO)

- [ ] **Reemplazar `https://dave-calderon.vercel.app/`** por el dominio final en TODA la página (find&replace global) — está en Schema.org, OG, canonical
- [ ] **Crear cuenta Google Business Profile** para Dave (#1 ROI para SEO local)
- [ ] **Conectar GA4** (descomenta líneas en `<head>`)
- [ ] **Conectar Meta Pixel** (descomenta líneas en `<head>`)
- [ ] **Crear `sitemap.xml`** básico
- [ ] **Foto secundaria de Dave** (`images/dave-about.jpg`) — cualquier foto en su entorno de trabajo, no necesariamente esmoquin
- [ ] **Validar Schema.org** en https://validator.schema.org
- [ ] **Verificar OG preview** en https://developers.facebook.com/tools/debug

### Iteraciones visuales pendientes (V0.2)

- [ ] Aplicar referentes Pinterest que Mateo aún tiene que mandar (animación + estilo)
- [ ] Posiblemente: animación on-scroll en hero (fade-in + slide), micro-interacciones en cards
- [ ] Posiblemente: video de fondo en hero (Dave hablando muteado, similar al manifiesto de Cordent)

### Decisiones pendientes con Mateo

- [ ] ¿Logo definitivo? Por ahora monograma "DC" en SVG inline (placeholder funcional, escalable, no requiere assets)
- [ ] ¿El email de Dave para que lleguen los leads vía Formspree?
- [ ] ¿Mencionamos explícitamente "1-800-NO-FAULT" como red afiliada en el footer? (decisión Mateo)
- [ ] ¿Estado fiscal/legal de Dave en USA? Define si necesitamos disclaimer adicional sobre nexos comerciales con abogados (FTC disclosure)

## 🔄 Flujo de iteración

1. Editas `index.html` localmente
2. Doble click al archivo → preview en browser
3. Cuando esté aprobado: push a GitHub → Vercel auto-deploya en ~30s

## 🌐 Deploy local rápido

Solo abre `index.html` en el browser (doble click). Si necesitas servidor local:

```bash
cd "/Users/mateocorredormontano/Desktop/Claude Code/dave-calderon"
python3 -m http.server 8000
# luego abre http://localhost:8000
```

## ⚠️ Aviso legal CRÍTICO

Dave NO es abogado. La página debe mantener este disclaimer en TODO momento:
- Banner top
- Hero
- Form (checkbox de consentimiento)
- Footer (caja destacada)
- gracias.html

USA tiene leyes anti-runner/capper (Florida, NY especialmente) que sancionan publicidad engañosa de servicios "casi-legales". Cualquier copy que sugiera que Dave da asesoría legal es riesgoso.

**Lenguaje OK:** "te conecto", "te pongo en contacto", "información sobre tus opciones"
**Lenguaje a evitar:** "te represento", "tu caso", "ganamos", "garantizamos resultados"

## 📐 Design system

CSS variables en `:root` permiten swap completo de paleta editando ~6 valores:

```css
--navy: #0F1F3D;       /* primary */
--gold: #D4A95C;       /* accent */
--brasa: #E8723D;      /* CTA secondary */
--cream: #F8F5EE;      /* bg */
--whatsapp: #25D366;   /* fixed */
--emergency: #DC2626;  /* call CTA */
```

Tipografía: Schibsted Grotesk (sans) + Fraunces (italic accent en titles).
