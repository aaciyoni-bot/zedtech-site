# ZedTech 📱⚡🇿🇲

**Gadgets & electronics at wholesale prices, paid for with Mobile Money.**

ZedTech is a mirror-store for AliExpress specialised in **phones, electronics and gadgets**
for Zambia: smartphones, earbuds, power banks, chargers, smartwatches, gaming and more —
priced in Zambian Kwacha (ZMW) and paid with **MTN MoMo, Airtel Money or Zamtel Kwacha**,
no credit card required.

Part of the **ZedMall / ZedGlow** family under **ORIZIS TECHNOLOGY**; built from the clean
`zedglow-site` template.

## Business model

Customer pays with Mobile Money → order lands in your WhatsApp **with a direct AliExpress
link for each item** → you re-order to the customer's address in a couple of taps →
drop-ship (12–25 days).

**Company profit = 30 % markup** on every item (`MARKUP_PERCENT: 30`), plus a 5 % service
fee at checkout that covers Mobile Money charges.

## Structure

Single-page storefront + a small backend.

| File | Purpose |
|------|---------|
| `index.html` | The whole storefront — catalog, cart, Mobile Money checkout. All config in the `CONFIG` block at the top of the `<script>`. |
| `backend/` | Express (Vercel serverless): `/api/products` proxies AliExpress via RapidAPI (key server-side), `/api/pay` + `/api/pay/status` drive pawaPay Mobile Money. |
| `manifest.json`, `sw.js`, `icon-*.png`, `og-cover.png` | PWA + social share preview. |

### CONFIG (top of `index.html`)

```js
API_BASE_URL: "https://zedtech-site.vercel.app",  // backend; "" = demo mode
USD_TO_ZMW: 27.5,          // exchange rate USD → Kwacha
MARKUP_PERCENT: 30,        // company profit on every purchase
SERVICE_FEE_PERCENT: 5,    // checkout fee (covers MoMo charges)
BUDGET_MAX_ZMW: 200,       // ceiling for the "Tech deals under K200" strip
WHATSAPP_ORDERS: "260971234567",
WHATSAPP_SUPPORT: "260971234567"
```

If the backend is unreachable (or has no `RAPIDAPI_KEY`), the storefront falls back to a
built-in set of **demo tech products** — it never shows a broken page.

## Conventions kept from the family (don't break these)

- **30 % markup** is required.
- **Never** mention "China" in visible copy — brand as *wholesale / imported / factory-direct*.
- **No floating WhatsApp button** — only the clean WhatsApp button inside the support section.
- WhatsApp orders include a **direct AliExpress link per item** (`aliLink`: numeric id →
  `https://www.aliexpress.com/item/ID.html`). Demo ids (e.g. `ph1`) get no link.
- Backend has **no hardcoded keys** — only `process.env.RAPIDAPI_KEY`.

## Deploy

**Storefront** (static, no build): push to `main` → GitHub Pages.

**Backend** (Vercel):
- Root Directory: `backend`
- Env vars: `RAPIDAPI_KEY` (host `aliexpress-datahub.p.rapidapi.com`), `PAWAPAY_TOKEN` +
  `PAWAPAY_ENV=production`.
- Health check: `GET /api/health`.

## Going live checklist

1. Add `RAPIDAPI_KEY` in Vercel → real product catalog replaces the demo items.
2. pawaPay account → `PAWAPAY_TOKEN` (sandbox → production) → real Mobile Money charges.
3. Set your real `WHATSAPP_ORDERS` / `WHATSAPP_SUPPORT` numbers in `CONFIG`.
4. Optional: custom domain via a `CNAME` file.
