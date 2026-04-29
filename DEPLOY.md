# AXON — Deployment Guide

## What's in this package

| File | URL (after deploy) | Purpose |
|---|---|---|
| `axon-landing.html` | `getaxon.io/` | Marketing page + waitlist |
| `axon-support-agent.html` | `getaxon.io/demo` | Live AI agent demo for closing clients |
| `axon-dashboard.html` | `getaxon.io/dashboard` | Operator control center |
| `axon-sales-agent.html` | `getaxon.io/sales` | Sales pipeline + AI compose |
| `vercel.json` | — | Route config |

---

## Step 1 — Get the domain

Buy `getaxon.io` (or `axon.ai`, `useaxon.io`) at Namecheap or Cloudflare Registrar.
Cloudflare Registrar is recommended — at-cost pricing, free DNS management.

---

## Step 2 — Deploy to Vercel

### Option A: Drag and drop (fastest — 2 minutes)
1. Go to [vercel.com](https://vercel.com) → New Project
2. Drag the entire `axon-deploy/` folder into the Vercel upload area
3. Vercel detects the `vercel.json` routes automatically
4. Click Deploy

### Option B: CLI
```bash
npm i -g vercel
cd axon-deploy/
vercel --prod
```

---

## Step 3 — Connect your domain

1. In Vercel → Project → Settings → Domains
2. Add `getaxon.io` and `www.getaxon.io`
3. Vercel gives you two DNS records (A + CNAME)
4. Add them in your domain registrar's DNS panel
5. Propagates in 5–30 minutes

---

## Step 4 — API Key (critical)

The Support Agent, Sales Compose, and Lead Analyzer all call the Anthropic API directly from the browser. This works for demos but exposes your API key.

**For demo use (right now):**
The files currently use the in-built Claude.ai API access — no key needed when running inside Claude artifacts. When deployed standalone, you need to add your key.

**To add your key for standalone deployment:**
In each HTML file, find the fetch call to `https://api.anthropic.com/v1/messages` and add the header:
```javascript
headers: {
  "Content-Type": "application/json",
  "x-api-key": "sk-ant-YOUR_KEY_HERE",
  "anthropic-version": "2023-06-01",
  "anthropic-dangerous-direct-browser-access": "true"
}
```

Get your key at: [console.anthropic.com](https://console.anthropic.com)

**For production (when you have paying clients):**
Move the API calls to a serverless function (Vercel Edge Function or a simple Express backend) so the key is never exposed client-side. A 20-line proxy function handles this.

---

## Step 5 — Collect waitlist signups

The landing page has a waitlist form. To capture emails:
1. Create a free [Airtable](https://airtable.com) base with columns: Email, Date, Source
2. Replace the `submitWaitlist()` function in `axon-landing.html` with a fetch POST to your Airtable API endpoint
3. Or use [Formspree](https://formspree.io) — paste your form endpoint, zero config

---

## Sharing links (before domain is live)

Vercel gives you an instant `.vercel.app` URL on deploy.
Use these for your first client demos while the domain propagates:

- `https://axon-xxxx.vercel.app/` → Landing
- `https://axon-xxxx.vercel.app/demo` → Live agent demo ← **send this to prospects**
- `https://axon-xxxx.vercel.app/dashboard` → Operator view
- `https://axon-xxxx.vercel.app/sales` → Sales pipeline

---

## First 10 clients — suggested outreach sequence

1. Deploy → get your `.vercel.app` demo link
2. Find 20 SMB targets (e-commerce, clinics, agencies, restaurants)
3. Send the demo link with this framing:
   > "Spent 2 minutes configuring this as your support agent — take a look: [link]. Curious what you think."
4. Let the product close for you
5. Charge $499–$2,400/month depending on plan

---

## Recommended stack additions (when you scale)

| Need | Tool | Cost |
|---|---|---|
| Auth for dashboard | Clerk.dev | Free tier |
| Email capture | Resend + Airtable | Free tier |
| Payments | Stripe | 2.9% per transaction |
| API proxy | Vercel Edge Functions | Included |
| Analytics | Plausible | $9/mo |

---

*Built with AXON · Trust is earned. We built it in.*
