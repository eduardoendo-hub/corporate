# Impacta para Empresas — Landing Page (Corporate)

LP institucional **Impacta para Empresas** — educação corporativa, certificações oficiais e IA.

- **Produção:** https://corporate.technowhub.ai → espelhada em https://www.impacta.com.br/corporate
- **Hospedagem:** Coolify (VPS Hetzner, `159.69.240.1`) — container nginx via `Dockerfile`.
- **IRIS:** produto `corporate` (cockpit em tempo real, `https://iris.technowhub.ai/api/events`).
- **WhatsApp:** `https://api.whatsapp.com/send?phone=551132548300` (mesmo número oficial da Impacta).

## Como funciona

`app.js`:
- captura os `utm_*` (+ `gclid`/`fbclid`) da URL e **persiste por sessão** — é assim que
  sabemos de qual campanha/anúncio o lead veio (atribuição para as campanhas);
- **repassa** os params aos links de saída absolutos (`a[data-cta]` — WhatsApp);
- emite eventos pra IRIS: `lp_view`, `click_whats`, `click_cta`, `lead`;
- trata o **formulário de lead** (validação → IRIS `lead` → RD Station → tela de sucesso).

Pixel Meta fica no-op até `CFG.META_PIXEL_ID`. Integração com **RD Station** fica no-op
até `CFG.RD_TOKEN` ser preenchido em `app.js` (etapa de integração com o RD).

## Deploy

Build estático servido por nginx (porta 80, healthcheck `/healthz`). Editar conteúdo →
commit → push → Coolify redeploya automático.
