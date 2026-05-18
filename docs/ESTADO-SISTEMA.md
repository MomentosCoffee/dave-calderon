# Estado del Sistema — Dave Calderón

> Inventario técnico completo. Actualizado: 18 mayo 2026.

---

## 🌐 Hosting + Dominio

| Item | Valor |
|---|---|
| **Repo GitHub** | github.com/MomentosCoffee/dave-calderon |
| **Hosting** | Vercel (auto-deploy desde GitHub main branch) |
| **Dominio actual** | dave-calderon.vercel.app |
| **Dominio futuro** | davecalderon.com (cuando justifique en mes 2-3) |
| **SSL** | ✅ Auto Vercel |
| **Páginas principales** | /, /privacy.html, /terminos.html, /gracias.html |

---

## 📊 Tracking + Analytics

| Servicio | ID | Estado |
|---|---|---|
| **Meta Pixel** | `3767126046761285` | ✅ LIVE en `<head>` de index.html |
| **Microsoft Clarity** | `wrddtybul2` | ✅ LIVE (heatmaps + session recordings) |
| **Google Analytics 4** | `REPLACE_GA4_ID` (pendiente) | ⏳ Crear si necesario para Google Ads futuro |

### Eventos Meta Pixel configurados

- `fbq('track', 'PageView')` — automático en todas las páginas
- `fbq('track', 'Lead', {value: 50, currency: 'USD', content_category: tipo_accidente})` — al submit del form
- `fbq('track', 'Contact', {content_name: 'phone_call_click'})` — click "Llamar"
- `fbq('track', 'Contact', {content_name: 'whatsapp_click'})` — click WhatsApp

---

## 📋 Formulario en página

Campos capturados:

```
nombre, telefono, tipo_accidente, fecha_accidente,
lugar_accidente, ciudad_vive, detalle, autorizacion
```

Hidden fields (auto):

```
utm_source, utm_medium, utm_campaign, utm_content,
referrer, page_url, submitted_at, user_agent, screen, viewport
```

Submit method: `fetch()` POST a webhook Make.com (NO Formspree).

---

## 🤖 Make.com Scenario

**Nombre**: Dave Calderon
**Plan**: Free tier (1000 ops/mes — vigilar consumo)
**Organization ID**: 3684680

### Módulos del scenario (orden de ejecución)

| # | Módulo | Tipo | Función |
|---|---|---|---|
| **1** | Webhooks Custom | Trigger | Recibe POST del form |
| **2** | Google Sheets Add a Row | Action | Guarda lead en CRM |
| **3** | HTTP POST | Action | Llama a Claude API (api.anthropic.com/v1/messages) — califica lead |
| **6** | JSON Parse JSON | Action | Extrae score/urgency/summary de respuesta Claude |
| **7** | Google Sheets Update a Row | Action | Actualiza fila con claude_score, summary, urgency, status="contactar" |
| **8** | HTTP POST | Action | WhatsApp Cloud API → mensaje AL LEAD |
| **10** | HTTP POST | Action | WhatsApp Cloud API → alerta a DAVE (+15162337514) |
| **12** | Gmail Send Email | Action | Email backup a Dave (juandacal@hotmail.com) + CC Mateo |

### Configuración crítica

- **Webhook URL**: `https://hook.us2.make.com/72cq59vugglj4qseygqmx5m0m8rnuhv1`
- **Anthropic API Key**: meterido en módulo 3 (header x-api-key)
- **WhatsApp Bearer Token**: meterido en headers Authorization de módulos 8 y 10 — **PERMANENTE (System User dave-makecom-bot)**
- **Connection Google Sheets**: cuenta `info.momentos.coffee@gmail.com`
- **Connection Gmail**: misma cuenta

---

## 📱 WhatsApp Cloud API

### App Meta Developer

- **App Name**: Dave Calderon Notifications
- **App ID**: 982264781449336
- **Business Portfolio**: REMAKE Agency (NO de Dave)

### WhatsApp Business Account

- **WABA ID**: 2004318527010424
- **Categoría**: Business
- **Estado**: Sandbox / Test

### Phone Numbers

| Tipo | Número | Phone Number ID | Notas |
|---|---|---|---|
| **Test (Meta)** | +1 555 153 0870 | `863431606854790` | Único que SE USA como sender hoy |
| **Real de Dave** | +1 516 233 7514 | N/A | Conectado como TESTER recipient (puede recibir mensajes). NO está como sender porque WhatsApp personal de Dave lo ocupa. |

### Tokens

| Token | Estado | Notas |
|---|---|---|
| **Token temporal (Paso 1 Pruébalo)** | ❌ Expirado (24h) | Se generaron varios, todos expiraron |
| **Token PERMANENTE (System User)** | ✅ ACTIVO | Empieza con `EAAN9XVdfqHgBRSn...` Guardado en módulos 8 y 10 de Make. NO expira. |

### System User

- **Nombre**: dave-makecom-bot
- **ID**: 61589494516480
- **Rol**: Admin
- **Activos asignados**:
  - App Dave Calderon (Control total)
  - WhatsApp Account 2004318527010424 (Control total)
- **Permisos token**: whatsapp_business_messaging + whatsapp_business_management
- **Expiración**: Never

### Limitaciones actuales

1. **Ventana 24h**: leads que NO escribieron primero al número de prueba NO reciben respuestas free-form
2. **Solo testers verificados**: el número de prueba solo manda a testers en la lista (Mateo + Dave verificados)
3. **Para producción real necesitamos**: templates aprobados de Meta (24-48h aprobación)

---

## 🧠 Claude API (Anthropic)

| Item | Valor |
|---|---|
| **Modelo usado** | claude-haiku-4-5 |
| **Endpoint** | https://api.anthropic.com/v1/messages |
| **API Key** | Guardada en módulo 3 de Make (header `x-api-key`) |
| **Saldo cargado** | $5 USD (alcanza ~2,500 leads) |
| **Costo por lead** | ~$0.002 USD |

### System prompt actual (módulo 3 Make)

Califica leads con score 0-100. Devuelve JSON con:
- `score` (0-100)
- `urgency` (alta/media/baja)
- `summary` (resumen 1-2 frases)
- `recommended_action` (qué hacer primero)
- `red_flags` (array)

Criterios principales:
- +30 si tipo es auto o trabajo
- +20 si estado USA cubierto
- +20 si detalle coherente
- +15 si teléfono parece USA
- +15 si accidente reciente
- -20 si menciona "consulta gratis" o "cuánto me toca"
- -50 si estado fuera de USA

### Pendiente actualizar

- Incluir campos nuevos del form en el prompt: `fecha_accidente`, `lugar_accidente`, `ciudad_vive`
- Agregar criterios: +10 si fecha últimos 30 días, -30 si más de 2 años

---

## 📊 Google Sheets CRM

| Item | Valor |
|---|---|
| **Nombre del Sheet** | Dave Calderon - CRM |
| **Cuenta dueño** | Mateo (info.momentos.coffee@gmail.com) |
| **Hoja** | Sheet1 |
| **Ubicación en Drive** | /Remake/Dave calderon/Dave Calderon - CRM |

### Columnas (en orden)

```
A: timestamp
B: nombre
C: telefono
D: tipo_accidente
E: lugar_accidente   (anteriormente "estado", renombrado)
F: detalle
G: fecha_accidente   (NUEVO)
H: ciudad_vive       (NUEVO)
I: utm_source
J: utm_medium
K: utm_campaign
L: page_url
M: referrer
N: user_agent
O: status            (valores: nuevo / contactar / cerrado / descartado)
P: claude_score      (0-100)
Q: claude_summary
R: claude_urgency    (alta/media/baja)
```

---

## 📧 Email (Gmail vía Make)

- **From**: mateo.corredor@helloremake.com
- **To**: juandacal@hotmail.com (Dave)
- **CC**: mateo.corredor@helloremake.com (Mateo monitorea)
- **Subject template**: `Lead nuevo Dave - {nombre} - Score {score}`
- **Body**: HTML con todos los datos del lead + análisis Claude

### Pendiente

- Actualizar template con campos nuevos: fecha_accidente, lugar_accidente, ciudad_vive

---

## 💰 Meta Ads (próximo a configurar)

### Lo que ya está

- ✅ Business Manager REMAKE con acceso completo
- ✅ Cuenta publicitaria de Dave creada (partnership existing partner "Dave Calderon")
- ✅ Pixel conectado a la cuenta publicitaria
- ✅ Página Facebook (verificar mañana en Zoom)

### Lo que falta MAÑANA

- ⏳ Conectar tarjeta de Mateo como payment method
- ⏳ Crear template WhatsApp aprobado (Meta tarda 24-48h)
- ⏳ Crear 3 creativos en Canva (specs en `docs/PAUTA-META-MAYO-2026.md`)
- ⏳ Configurar campaña Click-to-WhatsApp con Meta Advantage+
- ⏳ Audiencia: hispanos USA NY/NJ/FL/TX, 25-65, intereses específicos
- ⏳ Marcar Special Ad Categories → Social Issues (obligatorio para legal services)
- ⏳ Lanzar con $13/día = $200 USD por 15 días

---

## 🔒 Compliance legal cumplido

- ✅ `privacy.html` publicada — política de privacidad compliance TCPA
- ✅ `terminos.html` publicada — términos compliance anti-runner/capper laws
- ✅ Disclaimer "Dave NO es abogado" en hero, about, form, footer, WhatsApp auto-reply, gracias.html, emails
- ✅ Cláusula tracking de ventas en contrato Dave
- ✅ Email/teléfono captura con consentimiento explícito (checkbox required)

---

## 📁 Estructura del repo

```
dave-calderon/
├── CLAUDE.md                              ← Contexto para Claude futuro (este es prioridad)
├── README.md                              ← Readme técnico
├── index.html                             ← Página principal
├── privacy.html                           ← Política de privacidad
├── terminos.html                          ← Términos de servicio
├── gracias.html                           ← Página post-submit
├── robots.txt
├── vercel.json
├── images/                                ← Assets de imágenes (placeholder por ahora)
└── docs/
    ├── PAUTA-META-MAYO-2026.md            ← Plan completo de pauta (LEE PRIMERO MAÑANA)
    ├── SETUP-MAKECOM.md                   ← Setup original Make.com
    ├── ESTADO-SISTEMA.md                  ← Este archivo
    └── PROMPTS-IMAGENES.md                ← Prompts AI para imágenes
```

---

## 🚀 Próximos pasos inmediatos

### Mañana 18 mayo (día de lanzamiento)

1. **9am** — Mateo le manda link de pago a Dave ($200 USD vía PayPal)
2. **10am** — Cuando Dave pague, Zoom 30min:
   - Verificar/asociar Página de Facebook de Dave
   - Conectar tarjeta de Mateo a cuenta publicitaria
   - Crear template WhatsApp aprobado
3. **11am-1pm** — Mateo hace 3 visuales en Canva (specs en PAUTA doc)
4. **1:30pm** — Configurar campaña en Meta Ads Manager (sigue PAUTA doc)
5. **2pm** — LANZAR

### Mientras Meta optimiza (días 1-7)

- NO tocar la campaña (fase de aprendizaje)
- Monitor diario: CTR, CPM, conversaciones iniciadas
- Si pasan 7 días sin conversaciones: pausar peor creativo, agregar otro

### Día 8-15

- Primeros leads esperados (objetivo: 2-5 leads calificados)
- Dave atiende manualmente cada WhatsApp
- Documentar cada lead cerrado para validación

### Mes 2 (si funciona)

- Comprar dominio davecalderon.com
- Implementar webhook entrante de WhatsApp (capture inbound en CRM)
- Considerar Sofia conversacional (bot Claude perfila leads)
- Considerar más clientes con mismo modelo (Manuel, Yelitza, tíos)

---

## 🆘 Troubleshooting común

### Token WhatsApp falla con error 190

- Antes del System User: expiraba cada 24h, regenerar
- Ahora con System User: NO debería pasar. Si pasa, regenerar System User token (NO temporal)

### Lead llena form pero no llega a Make

- Verificar webhook URL en form (data-webhook attribute en index.html línea ~1640)
- Verificar Make scenario está activado (toggle ON)

### Claude API responde con markdown ` ```json...``` `

- Sistema actual ya tiene `replace()` en módulo 6 (Parse JSON) para limpiar
- Si vuelve a pasar, verificar prompt del módulo 3 tiene "responde sin markdown"

### Sheets no actualiza con score Claude

- Verificar módulo 5 (Update Row) tiene `Row number` apuntando a módulo 2 → `Row number`
- Verificar columnas P, Q, R están mapeadas a módulo Parse JSON (no a otro)

---

## 📞 Información de contacto

| Persona | Rol | Contacto |
|---|---|---|
| **Mateo Corredor** | Dueño Remake (operador) | mateo.corredor@helloremake.com / +1 587 585 4143 (Calgary) |
| **Dave Calderón** | Cliente final | juandacal@hotmail.com / +1 516 233 7514 (NY) |

---

**Última actualización**: 18 mayo 2026 — Sistema 100% funcional. Día de lanzamiento de pauta.
