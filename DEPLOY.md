# AXON — Full Deployment Guide v2

## Complete file manifest

| File | URL | Purpose |
|---|---|---|
| `axon-landing.html` | `/` | Marketing page + waitlist |
| `axon-support-agent.html` | `/demo` | Live 3-agent demo |
| `axon-dashboard.html` | `/dashboard` | Operator control center |
| `axon-sales-agent.html` | `/sales` | Sales pipeline |
| `axon-onboard.html` | `/onboard` | Client onboarding flow |
| `axon-widget-preview.html` | `/widget` | Embed widget preview |
| `axon-pricing.html` | `/pricing` | Pricing + Stripe checkout |
| `api/chat.js` | `/api/chat` | Anthropic API proxy |
| `vercel.json` | — | Route config |

---

## Step 1 — Deploy to Vercel

```bash
npm i -g vercel
cd axon-deploy/
vercel --prod
```
Or drag the folder into vercel.com/new.

---

## Step 2 — Add Anthropic API key

1. console.anthropic.com → API Keys → Create Key
2. Vercel → Project → Settings → Environment Variables
3. Add: `ANTHROPIC_API_KEY` = `sk-ant-your-key-here`
4. Redeploy

---

## Step 3 — Wire Formspree (waitlist)

1. formspree.io → New Form → copy your Form ID
2. In axon-landing.html, replace `YOUR_FORM_ID`

---

## Step 4 — Stripe payment links

1. dashboard.stripe.com → Payment Links → Create
2. AXON Starter — $499/month recurring
3. AXON Growth — $999/month recurring
4. In axon-pricing.html replace YOUR_STARTER_LINK and YOUR_GROWTH_LINK
5. Set Stripe success URL to: https://yourdomain.com/onboard

---

## Step 5 — Custom domain

1. Buy getaxon.io on Cloudflare Registrar
2. Vercel → Project → Settings → Domains → Add getaxon.io
3. Add the A + CNAME records Vercel gives you to your DNS
4. Live in 5-30 minutes

---

## Full URL map

| URL | Who |
|---|---|
| getaxon.io | Prospects |
| getaxon.io/demo | Prospects to close |
| getaxon.io/pricing | Ready to buy |
| getaxon.io/onboard | After payment |
| getaxon.io/dashboard | You (operator) |
| getaxon.io/sales | You (leads) |
| getaxon.io/widget | Widget demo |

---
AXON · Trust is earned. We built it in.
