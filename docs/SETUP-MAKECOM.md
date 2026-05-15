# Setup completo de Make.com para Dave — Speed-to-lead <30s

Esta guía te lleva de cero al sistema funcionando en 30-45 minutos.

## Stack final

```
Form en página
   ↓ POST JSON
Make.com webhook
   ↓ (en paralelo, todo en <10 segundos):
   ├─→ Google Sheets (CRM)
   ├─→ WhatsApp Cloud API → auto-reply al LEAD ("Recibimos tu info, te llamamos pronto")
   ├─→ WhatsApp Cloud API → alerta a DAVE en su celular ("Lead nuevo: Juan, NY, accidente auto")
   ├─→ Email a Dave con resumen formateado
   └─→ Claude Sonnet 4.5 → califica el lead (score 0-100, urgencia, viabilidad)
```

## Cuentas que necesitas crear (15 min)

### 1. Make.com (5 min)

1. Ir a [make.com/en/register](https://www.make.com/en/register)
2. Sign up con tu Google (mateo.corredor@helloremake.com)
3. Plan: **Free** — 1,000 operaciones/mes (alcanza para ~50-80 leads)
4. Cuando llegues a 50 leads, subes a Core ($9/mes, 10,000 ops)

### 2. WhatsApp Cloud API (10 min) — la magia que mata Formspree

Esta es la diferencia entre "agencia 2024" y "agencia 2026". Meta lo da gratis.

1. Ir a [developers.facebook.com](https://developers.facebook.com)
2. Login con tu Facebook personal
3. **My Apps** → **Create App** → **Other** → **Business**
4. Nombre: `Dave Calderon Notifications`
5. En el dashboard: agregar producto **WhatsApp** → **Set Up**
6. Va a darte:
   - **Phone number ID** (un número de prueba de Meta para empezar)
   - **WhatsApp Business Account ID**
   - **Access token** (temporal por 24h — después generamos uno permanente)

**Gratis**: 1,000 conversaciones inbound/mes + ilimitadas a tu propio número de prueba.

**Cuando Dave esté activo**: agregamos el número real de Dave (+1 516-233-7514) como sender. Eso requiere verificación de business (paso aparte, 2-3 días de revisión Meta).

### 3. Microsoft Clarity (2 min)

1. Ir a [clarity.microsoft.com](https://clarity.microsoft.com)
2. Sign in con Microsoft o Google
3. **Add new project** → URL: `https://dave-calderon.vercel.app`
4. Te da un **Project ID** (algo tipo `qx7n8m9pXYZ`)
5. Reemplazar `REPLACE_CLARITY_ID` en `index.html` con ese ID

**Esto te da gratis**: heatmaps, session recordings, "rage clicks", "dead clicks". Es lo que vas a mostrar a Dave en el reporte del lunes.

### 4. Google Sheets (2 min)

1. Ir a [sheets.new](https://sheets.new)
2. Renombrar a `Dave Calderón - CRM`
3. Headers en fila 1:
   ```
   A: timestamp
   B: nombre
   C: telefono
   D: tipo_accidente
   E: estado
   F: detalle
   G: utm_source
   H: utm_medium
   I: utm_campaign
   J: page_url
   K: referrer
   L: user_agent
   M: status (manual: nuevo / contactado / abogado_asignado / cerrado / descartado)
   N: claude_score (0-100)
   O: claude_summary
   P: claude_urgency (alta/media/baja)
   ```
4. Compartir con `mateo.corredor@helloremake.com` y con el email de Dave

### 5. Claude API key (3 min)

1. Ir a [console.anthropic.com](https://console.anthropic.com)
2. Sign up (si no tienes cuenta)
3. **Settings** → **API Keys** → **Create Key** → `dave-calderon-bot`
4. Guardar la key en un sitio seguro (no la podrás ver de nuevo)
5. Cargar $5 USD de saldo (alcanza para ~2,000 calificaciones de lead con Claude Haiku 4.5)

## Build del scenario en Make.com (20 min)

### Estructura visual del scenario

```
[Webhook] → [Aggregator] → [Router]
                              ├─→ [Google Sheets: Add Row]
                              ├─→ [HTTP: Anthropic Claude API]  → [Google Sheets: Update Row con score]
                              ├─→ [HTTP: WhatsApp Cloud → Lead]
                              ├─→ [HTTP: WhatsApp Cloud → Dave]
                              └─→ [Email: Resend o Gmail → Dave]
```

### Paso 1: Crear el webhook (1 min)

1. Make.com → **Create a new scenario**
2. Add module → **Webhooks** → **Custom webhook**
3. **Add** → nombre: `dave-form-webhook`
4. **Copy address to clipboard** — te da algo como:
   ```
   https://hook.us2.make.com/abc123xyz...
   ```
5. **Esta URL es la que pegas en `index.html`** reemplazando `REPLACE_WITH_MAKECOM_WEBHOOK_URL`

### Paso 2: Probar webhook desde la página (1 min)

1. En Make.com el webhook quedará en estado "Waiting for data"
2. En tu navegador, abre la página real y llena el form de prueba
3. Make.com detectará el payload y aprenderá la estructura
4. **Re-determine data structure** → confirma

### Paso 3: Agregar Google Sheets - Add a Row (3 min)

1. Add module → **Google Sheets** → **Add a Row**
2. Connect tu Google account
3. Spreadsheet: `Dave Calderón - CRM`
4. Sheet: `Hoja 1`
5. Mapear los campos del webhook a las columnas:
   - timestamp: `{{1.submitted_at}}`
   - nombre: `{{1.nombre}}`
   - telefono: `{{1.telefono}}`
   - tipo_accidente: `{{1.tipo_accidente}}`
   - estado: `{{1.estado}}`
   - detalle: `{{1.detalle}}`
   - utm_source: `{{1.utm_source}}`
   - utm_medium: `{{1.utm_medium}}`
   - utm_campaign: `{{1.utm_campaign}}`
   - page_url: `{{1.page_url}}`
   - referrer: `{{1.referrer}}`
   - user_agent: `{{1.user_agent}}`
   - status: `nuevo`

### Paso 4: WhatsApp auto-reply al LEAD (4 min)

1. Add module → **HTTP** → **Make a request**
2. URL: `https://graph.facebook.com/v21.0/{PHONE_NUMBER_ID}/messages`
   - Reemplaza `{PHONE_NUMBER_ID}` con el de tu app
3. Method: `POST`
4. Headers:
   ```
   Authorization: Bearer {TU_ACCESS_TOKEN}
   Content-Type: application/json
   ```
5. Body (JSON):
   ```json
   {
     "messaging_product": "whatsapp",
     "to": "{{1.telefono}}",
     "type": "text",
     "text": {
       "body": "Hola {{1.nombre}} 👋\n\nSoy Dave Calderón. Recibí tu información sobre el accidente. Te voy a llamar en los próximos minutos (máximo 1 hora) para escucharte y ver cómo te puedo ayudar.\n\nSi necesitas hablar ya: 516-233-7514\n\n⚠️ Importante: NO soy abogado. Te oriento y te conecto con uno especializado en tu estado."
     }
   }
   ```

### Paso 5: WhatsApp alerta a DAVE (4 min)

Mismo HTTP module pero:
- `to`: `15162337514` (sin +, sin guiones — el número de Dave)
- Body:
  ```json
  {
    "messaging_product": "whatsapp",
    "to": "15162337514",
    "type": "text",
    "text": {
      "body": "🚨 LEAD NUEVO\n\n👤 {{1.nombre}}\n📞 {{1.telefono}}\n🚗 Tipo: {{1.tipo_accidente}}\n📍 Estado: {{1.estado}}\n\n💬 Detalle:\n{{1.detalle}}\n\n🔗 Fuente: {{1.utm_source}} / {{1.utm_campaign}}\n\n⏰ Llamar en <1h. CRM actualizado en Sheets."
    }
  }
  ```

### Paso 6: Claude API para calificar el lead (5 min)

1. Add module → **HTTP** → **Make a request**
2. URL: `https://api.anthropic.com/v1/messages`
3. Method: `POST`
4. Headers:
   ```
   x-api-key: TU_CLAUDE_API_KEY
   anthropic-version: 2023-06-01
   Content-Type: application/json
   ```
5. Body:
   ```json
   {
     "model": "claude-haiku-4-5",
     "max_tokens": 500,
     "messages": [{
       "role": "user",
       "content": "Eres asistente de Dave Calderón, conector post-accidente en USA (NO es abogado). Califica este lead potencial en formato JSON estricto.\n\nDatos del lead:\n- Nombre: {{1.nombre}}\n- Teléfono: {{1.telefono}}\n- Tipo accidente: {{1.tipo_accidente}}\n- Estado USA: {{1.estado}}\n- Detalle: {{1.detalle}}\n\nDevuelve SOLO un objeto JSON con esta estructura exacta (sin texto adicional, sin markdown):\n{\n  \"score\": <0-100, donde 100 es lead PERFECTO listo para conectar con abogado>,\n  \"urgency\": <\"alta\" | \"media\" | \"baja\">,\n  \"summary\": <resumen de 1-2 frases del caso>,\n  \"recommended_action\": <qué debería hacer Dave primero>,\n  \"red_flags\": <array de problemas detectados, o array vacío>\n}\n\nCriterios de score:\n- Tipo es 'auto' o 'trabajo' (Dave SOLO maneja esos): +30\n- Estado está en cobertura USA: +20\n- Detalle es coherente y específico: +20\n- Teléfono parece válido USA: +15\n- Accidente reciente (último mes): +15\n- Si menciona 'consulta gratis' o 'cuánto me toca': -20 (cliente quiere asesoría legal, no es para Dave)\n- Si menciona estado fuera de USA: -50"
     }]
   }
   ```
6. Parse response → extraer JSON de `content[0].text`

### Paso 7: Actualizar Sheets con el score de Claude (2 min)

1. Add module → **Google Sheets** → **Update a Row**
2. Buscar fila por `nombre + timestamp`
3. Actualizar columnas `claude_score`, `claude_summary`, `claude_urgency`

### Paso 8: Email a Dave con resumen (3 min)

Si no quieres configurar Resend, usa Gmail directo:

1. Add module → **Email** → **Send an Email**
2. Connect Gmail (puede ser el de Mateo)
3. To: email de Dave
4. Subject: `🚨 Lead nuevo Dave: {{1.nombre}} - {{1.estado}}`
5. Content (HTML):
   ```html
   <h2>Nuevo lead capturado</h2>
   <p><strong>Score Claude:</strong> {{score_de_claude}}/100 — Urgencia: {{urgency}}</p>
   <hr>
   <p><strong>Nombre:</strong> {{1.nombre}}</p>
   <p><strong>Teléfono:</strong> <a href="tel:{{1.telefono}}">{{1.telefono}}</a></p>
   <p><strong>WhatsApp:</strong> <a href="https://wa.me/{{1.telefono}}">Abrir chat</a></p>
   <p><strong>Tipo accidente:</strong> {{1.tipo_accidente}}</p>
   <p><strong>Estado:</strong> {{1.estado}}</p>
   <p><strong>Detalle:</strong></p>
   <p>{{1.detalle}}</p>
   <hr>
   <p><strong>Resumen Claude:</strong> {{summary}}</p>
   <p><strong>Recomendación:</strong> {{recommended_action}}</p>
   <hr>
   <p>Ver en Sheets: {LINK_AL_SHEET}</p>
   ```

### Paso 9: Activar scheduling (1 min)

1. En el editor de Make: toggle **ON** en la esquina inferior izquierda
2. Schedule: **Immediately** (cada vez que llegue webhook)
3. Save

## Test end-to-end (5 min)

1. Abre `dave-calderon.vercel.app` en una pestaña incógnita
2. Llena el form con datos de prueba (usa TU teléfono y nombre)
3. Submit
4. En <30 segundos deberías ver:
   - ✅ Mensaje WhatsApp llegando a tu teléfono ("Recibí tu info...")
   - ✅ Mensaje WhatsApp llegando a Dave ("🚨 Lead nuevo...")
   - ✅ Nueva fila en Google Sheets con todos los datos
   - ✅ Email en tu inbox y el de Dave
   - ✅ En 5-10s extra: columna `claude_score` llenándose con número

## Costo total mensual

| Servicio | Free tier | Cuándo upgradear |
|---|---|---|
| Make.com | 1,000 ops/mes ($0) | A 50 leads/mes → $9/mes |
| WhatsApp Cloud API | 1,000 conversations/mes ($0) | A 500 leads/mes → ~$0.005 por extra |
| Microsoft Clarity | Ilimitado ($0) | Nunca, es gratis siempre |
| Google Sheets | Ilimitado ($0) | Cuando pase de 5,000 filas → migrar a Supabase |
| Claude Haiku 4.5 | Pay per use | ~$0.002 por lead calificado |
| **Total** | **~$0/mes para los primeros 50 leads** | |

## Próximas mejoras (después de tener 10+ leads de data real)

1. **Cal.com self-hosted** → Dave puede mandar link de booking dentro del WhatsApp auto-reply
2. **PostHog** → A/B testing del copy + heatmaps avanzados
3. **n8n self-hosted** → migrar de Make.com si pasamos de 500 leads/mes ($0/mes para siempre)
4. **Supabase + Retool** → CRM real con vista para Dave en su celular
5. **WhatsApp Flows** → form completo dentro de WhatsApp (sin salir al web)

## Lo que necesito de ti AHORA para activar

Cuando termines los pasos 1-9 de Make.com, pásame:
1. ✅ Tu webhook URL (la del paso 1)
2. ✅ Tu Microsoft Clarity Project ID
3. ✅ Tu Meta Pixel ID (cuando Dave dé acceso)
4. ✅ Tu GA4 Measurement ID (cuando crees property)

Yo reemplazo los `REPLACE_*` en el HTML, commiteo, y Vercel redespliega en 30s. Sistema activo.
