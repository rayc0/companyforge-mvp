# CompanyForge

> Run a portfolio of companies with AI agents — multi-tenant, audit-logged, with compliance-grade approval gates.

**Live demo:** https://companyforge.ai (mirror: https://companyforge-mvp.pages.dev)

CompanyForge is the autonomous operations platform built by a founder running 8 companies with 0 employees. It is the system, productized.

## What this MVP is

A single static HTML page that lets a stranger try the full CompanyForge experience in under 60 seconds, with **zero signup, zero backend, and zero data leaving their browser**.

- **Bring your own Anthropic API key** — stored only in `localStorage`, sent only to `api.anthropic.com`
- **8 preset companies** (Pinnacle, SeniorDeli, CapyPay, CompanyForge, LinPig, PumpHQ, Botzan, GoldRushX) — or add your own
- **Per-company memory injection** — every task you run for a company is automatically prefixed with that company's context
- **Band A / B / C / D approval gates**:
  - **A** — runs autonomously (research, summarize, draft internal notes)
  - **B** — runs and notifies (update dashboards, organize files)
  - **C** — drafts and **waits for human approval** (send email, publish, place order)
  - **D** — **hard-blocked at the client**, never even calls the API (sign contract, make payment, wire transfer)
- **Live savings counter** — labor minutes per task, summed and displayed in HKD
- **Per-job audit log** — every state transition with timestamp + actor
- **Streaming output** — live token-by-token via Anthropic SSE
- **Tenant isolation by browser** — each browser is its own tenant, no cross-contamination

## Portfolio in production (live proof)

CompanyForge is not vapor — it's the system the founder uses to run a live multi-venture portfolio. Every site below is **autonomously maintained by AI content officers** running on this exact pattern (per-company memory + role file + content queue + launchd schedule + audit log):

| Venture | URL | Vertical | Daily AI run |
|---|---|---|---|
| **SeniorDeli (软餐)** | [softmeal.org](https://softmeal.org) | Dysphagia / care food encyclopedia | 10:00 HKT |
| **DSE 升學知識庫** | [dsedaquan.cn](https://dsedaquan.cn) | 大陸學生赴港升學 | 09:30 HKT |
| **药油大全** | [yaoyoudaquan.cn](https://yaoyoudaquan.cn) | Global medicated oil encyclopedia | 11:00 HKT |
| **CompanyForge** | [companyforge.ai](https://companyforge.ai) | The AI operator platform itself | continuous |

Each hub publishes long-form, primary-source-cited articles every day with no human editing. The same approval-band model in this MVP is what gates publishing decisions in production:
- **Band A** — research, drafting, internal notes (autonomous)
- **Band B** — pushing markdown to GitHub Pages (autonomous + audit)
- **Band C** — adding new ventures, changing schedule (human approval)
- **Band D** — DNS / billing / contracts (hard-blocked at client)

This is CompanyForge dogfooding itself: 1 founder, 0 employees, 4+ live web properties, daily AI-authored content under editorial firewall + commercial conversion layer. Full content is CC BY 4.0 — auditable by anyone.

---

## Architecture

```
┌────────────────────┐         ┌──────────────────────┐
│  Your browser      │  HTTPS  │  api.anthropic.com   │
│  (localStorage)    │ ──────► │  (your API key)      │
│                    │         │                      │
│  • tenant id       │         └──────────────────────┘
│  • API key         │
│  • companies       │
│  • jobs + audit    │
└────────────────────┘

         ▲
         │  static HTML
         │
┌────────────────────┐
│  Cloudflare Pages  │
│  (no backend, no   │
│   database, no     │
│   tracking)        │
└────────────────────┘
```

There is no CompanyForge server. There is no database. We physically cannot see your tasks, your companies, or your API key.

## Why this is different from a Claude playground

| | Generic Claude playground | CompanyForge MVP |
|---|---|---|
| Multi-company portfolio view | ❌ | ✅ |
| Per-company memory auto-injection | ❌ | ✅ |
| Approval bands (A/B/C/D) | ❌ | ✅ |
| Compliance audit log per job | ❌ | ✅ |
| Labor savings counter (HKD) | ❌ | ✅ |
| Hard-block on regulated actions | ❌ | ✅ |
| Founder-portfolio framing | ❌ | ✅ |

## Run locally

```bash
cd companyforge-mvp
python3 -m http.server 8765
open http://localhost:8765/
```

That's it. No `npm install`, no build step.

## Deploy

```bash
wrangler pages deploy . --project-name=companyforge-mvp
```

Or open `index.html` from any static host (GitHub Pages, Vercel, Netlify, S3, an iPad).

## Status

- v0.1 — MVP, 14/14 Playwright headless tests passing
- Built 2026-04-10
- Roadmap: real auth + workspaces (v0.2), Lark/WeCom integration (v0.3), team plans (v0.4)

## License

TBD — currently all rights reserved.
