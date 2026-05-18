# Dave Calderón — Proyecto

> **Cualquier Claude que abra este repo debe leer este archivo PRIMERO.**

## Quién es Dave Calderón

- **Nombre legal**: David Felipe Calderón Galeano
- **DBA**: Dave Calderón
- **Ubicación**: 183 Madison Ave, Manhattan, NY
- **WhatsApp/teléfono**: +1 516-233-7514
- **Email**: juandacal@hotmail.com (cuidado: en el formulario lo escribió mal como `juandacal@hotmalm.com` — typo confirmado, el correcto es hotmail)
- **Negocio**: Servicio de orientación y conexión post-accidente en USA
- **CRÍTICO**: Dave **NO es abogado**. NO da asesoría legal. Solo orienta y conecta con abogados independientes. Esto debe estar en TODOS los puntos de contacto (web, WhatsApp, email, ads).
- **Tipo de casos**: Solo accidentes de **auto** + accidentes de **trabajo** (workers comp)
- **Estados que cubre**: 34 estados USA (lista completa en `index.html` sección coverage)
- **Idioma**: **Solo español** (confirmado en formulario, NO bilingüe)
- **Tiempo de respuesta promesa**: 15 minutos a 1 hora

## Quién es Mateo Corredor (dueño Remake)

- **Dueño de Remake** (agencia de marketing digital, 12 años, 60+ clientes históricos)
- **Ubicación**: Calgary, Canadá (+1 587 585 4143)
- **Email negocio**: mateo.corredor@helloremake.com
- **Email Gmail personal**: mateocorredor145@gmail.com
- **Email cuenta Momentos (donde está logueado MCP Claude)**: info.momentos.coffee@gmail.com
- **Cuenta GitHub**: MomentosCoffee
- **Beginner coder** (~2 meses), Mac nuevo sin Node/brew/npm instalados
- **Habla español** — prefiere todo en español

## Stack técnico

```
Lead llena form en dave-calderon.vercel.app
   ↓
Webhook → Make.com (https://hook.us2.make.com/72cq59vugglj4qseygqmx5m0m8rnuhv1)
   ↓ (8 módulos en paralelo)
   ├─→ Google Sheets "Dave Calderon - CRM" (Add Row)
   ├─→ HTTP Anthropic Claude API (califica lead score 0-100)
   ├─→ JSON Parse (extrae score/urgency/summary)
   ├─→ Google Sheets (Update Row con score Claude)
   ├─→ HTTP WhatsApp Cloud API (mensaje al LEAD)
   ├─→ HTTP WhatsApp Cloud API (alerta a DAVE +15162337514)
   └─→ Gmail (email backup a Dave + Mateo CC)
```

## IDs y configs críticos

> Detalles completos en `docs/ESTADO-SISTEMA.md`

- **Meta App**: Dave Calderon Notifications (ID 982264781449336)
- **WhatsApp Business Account ID (WABA)**: 2004318527010424
- **WhatsApp Phone Number ID (test)**: 863431606854790
- **Número WhatsApp test de Meta**: +1 555 153 0870
- **Número WhatsApp real Dave** (NO conectado como sender todavía): +1 516-233-7514
- **Meta Pixel ID**: 3767126046761285 (LIVE en página)
- **Microsoft Clarity ID**: wrddtybul2 (LIVE)
- **Anthropic API Key**: usado en Make.com módulo 3 (NO hardcoded en código)
- **Make.com Org ID**: 3684680
- **Business Portfolio**: REMAKE (NO el de Dave — Mateo opera todo desde REMAKE BM)
- **Cuenta publicitaria**: creada en REMAKE BM con partnership Dave Calderon (conectada al Pixel)
- **Token WhatsApp PERMANENTE** (System User dave-makecom-bot): activo, no expira

## Decisiones críticas tomadas

### 1. **Stack manual sobre HighLevel** ✅
- Para 1-3 clientes: Make.com + Claude + Sheets más barato y flexible
- HighLevel solo justifica con 5+ clientes pagando $1500+/mes

### 2. **Solo español, NO bilingüe** ✅
- Dave dijo "solo español" en formulario
- Página actual es 100% español (anteriormente era bilingüe ES/EN, lo cambiamos)

### 3. **Opción C — Número de prueba Meta como sender** ✅
- NO desconectar el WhatsApp personal de Dave (+15162337514) de su celular
- Mensajes salen del número de prueba de Meta (+1 555 153 0870)
- En el mensaje al lead, número de Dave está destacado para que lo guarde
- Cuando Dave decida si comprar número virtual nuevo o no, evaluamos cambio

### 4. **Cuenta publicitaria en REMAKE BM con partnership Dave** ✅
- Mateo no tiene cupo para crear cuenta publicitaria nueva en su REMAKE BM (límite Meta alcanzado)
- Solución: crear cuenta publicitaria asociada a "Dave Calderon" (existing partner) DENTRO de REMAKE BM
- Beneficio: anuncios aparecen como "Por Dave Calderón", pero Mateo opera todo desde su entorno

### 5. **Click-to-WhatsApp Ads, NO landing page tradicional para pauta** ✅
- Para hispanos USA, conversión 3-5x mejor que form web
- Landing sigue siendo asset principal (organic, SEO, credibilidad, backup)
- Pauta paga va directo a WhatsApp

### 6. **Manual atención de leads para arrancar** ✅
- Dave atiende WhatsApps manualmente los primeros leads
- Webhook entrante + Sofia conversacional → mes 2 si validamos volumen
- No invertir en automatización antes de validar demanda

### 7. **Token WhatsApp permanente vía System User** ✅
- Resuelve el problema de tener que regenerar cada 24h
- System User `dave-makecom-bot` en REMAKE BM, expiración "Nunca"
- Tiene permisos: whatsapp_business_messaging + whatsapp_business_management

### 8. **Modelo de pago Dave → Mateo (NO al revés)** ✅
- Dave paga adelantado a Mateo (PayPal/tarjeta)
- Mateo pone su tarjeta en Meta Ads
- Si optimiza y queda sobrante después de 2 leads efectivos, sobrante = bonificación Remake
- Russ approved: Mateo NO es banco

### 9. **Dominio Vercel temporal, davecalderon.com cuando justifique** ✅
- dave-calderon.vercel.app para arrancar
- Comprar davecalderon.com en mes 2 si llegan 5+ leads cerrados

### 10. **Privacy + Términos creados y publicados** ✅
- Necesario para Meta Ads compliance (legal services)
- /privacy.html + /terminos.html live
- Footer del index los linkea

## Reglas Russ aplicadas

- ❌ NO ser banco (Dave paga adelantado)
- ❌ NO complicar lo que funciona (mantener stack actual hasta que crezca)
- ❌ NO comprar herramientas innecesarias (AdCreative.ai, HighLevel, etc. — solo cuando justifique)
- ❌ NO automatizar lo que no se ha validado (webhook entrante solo cuando hay volumen)
- ✅ SÍ disciplina en costos (todo medido vs valor)
- ✅ SÍ documentar TODO (compliance + IP)
- ✅ SÍ aprovechar IA agresivamente (Claude API en módulo 3 califica leads)

## Compliance legal USA cumplido

- ✅ Disclaimer "NO es abogado" en hero, about, form, footer, WhatsApp, email
- ✅ Privacy Policy publicada
- ✅ Términos de Servicio publicados
- ✅ Compliance TCPA, anti-runner/capper laws (FL/NY/TX), FTC, Meta Ads legal services
- ✅ Cláusula de tracking de ventas en contrato Dave
- ⏳ Special Ad Categories — se marca al crear campaña (mañana 18 mayo)

## Estado actual (17-18 mayo 2026)

### ✅ HECHO

- Sistema completo end-to-end funcionando (8 módulos Make verdes)
- Dave recibe leads en WhatsApp + Email
- Token WhatsApp PERMANENTE (no más renovaciones cada 24h)
- Meta Pixel LIVE en la página (ID 3767126046761285)
- Privacy + términos publicados
- Cuenta publicitaria de Dave creada en REMAKE BM
- Plan de pauta documentado (`docs/PAUTA-META-MAYO-2026.md`)

### ⏳ PARA MAÑANA (DÍA DE LANZAMIENTO)

1. Cobrar a Dave $200 USD (PayPal/tarjeta)
2. Crear template WhatsApp aprobado por Meta (24-48h espera)
3. Conectar tarjeta Mateo a cuenta publicitaria Dave (5 min)
4. Crear 3 visuales Canva siguiendo `docs/PAUTA-META-MAYO-2026.md`
5. Configurar Click-to-WhatsApp Ads con Meta Advantage+ (sigue el doc)
6. Lanzar campaña $13/día por 15 días

### 🎯 OBJETIVO

**2 leads efectivos con $200 USD** antes de fin de mes (~15 días pauta).

Si NO se logra: Dave puede no pagar más → modelo no escala.
Si SÍ se logra: validación → escalar a $500-1000/mes en mes 2 → Sofia conversacional → bot Meta Ads → más clientes.

## Documentación relacionada

- `README.md` — readme del proyecto técnico
- `docs/PAUTA-META-MAYO-2026.md` — plan completo de pauta (copies, audiencia, Canva, config Meta)
- `docs/SETUP-MAKECOM.md` — setup original de Make.com
- `docs/ESTADO-SISTEMA.md` — inventario técnico detallado (IDs, configs)
- `docs/PROMPTS-IMAGENES.md` — prompts AI para generar imágenes de Dave

## Cómo retomar mañana

Mateo abre Claude Code en este directorio (`dave-calderon/`):

```bash
cd "/Users/mateocorredormontano/Desktop/Claude Code/dave-calderon"
claude
```

Primer mensaje al nuevo Claude:
> "vamos a lanzar la pauta hoy, sigue el plan de docs/PAUTA-META-MAYO-2026.md"

El nuevo Claude ya tiene todo el contexto leyendo este archivo. Cero info que pegar.
