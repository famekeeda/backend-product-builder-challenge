# 🧪 Fame Keeda — Backend Product Builder Challenge

Build backend systems that power real-time influencer marketing, campaign orchestration, and AI-driven automation. This is your chance to show us how you think, not just how you code.

### 📝 Description:
This repo contains the technical assessment for candidates applying for the **Backend/API Developer (AI Integration)** role at Fame Keeda. The challenge is designed to simulate real-world use cases we handle at scale — spanning campaign workflows, LLM integrations, and streaming influencer analytics.

---

## 🎯 Challenge Overview

We’re scaling **Fame Dash** and **Fame Konnect**—AI-powered platforms connecting brands with creators through smart orchestration, predictive insights, and trend automation. This challenge is your opportunity to demonstrate how you'd contribute to building the tech behind that.

It’s not about doing everything. It’s about doing **two things really well.**

---

## 🧩 Choose-Your-Own-Adventure: Pick Any 2 Sections

| Track | Goal | Est. Time |
|-------|------|-----------|
| 🌀 **A. Campaign Orchestrator API** | Microservices that trigger events, fire webhooks, retry failed actions, and maintain state | 4 hrs |
| 💡 **B. AI Brief Generator** | Use an LLM + tools/function-calling to generate a campaign brief | 3 hrs |
| 📊 **C. Streaming Metrics Pipeline** | Process influencer engagement stats in real-time; expose analytics API | 4 hrs |
| 📈 **D. Batch ETL + Top Performer API** | Parse 10k CSV rows, compute engagement scores, expose top-performers | 3 hrs |

---
---
## 🌀 Section A – Campaign Orchestrator Service
**Objective**  
Own the campaign lifecycle and deliver webhooks reliably.

| Area | Spec |
|------|------|
| **Core Endpoints** | `POST /campaigns` (create) • `PATCH /campaigns/:id` (update) |
| **Event Bus** | Publish `CampaignCreated` / `CampaignUpdated` to Kafka ⬌ RabbitMQ ⬌ Redis Streams |
| **Webhook Worker** | POST each event to `https://mock.endpoint/campaign` <br>• 3‑try exponential back‑off <br>• Dead‑Letter Queue after final failure |
| **Audit Log** | Persist `{ id, eventType, campaignId, attempt, status, timestamp }` |
| **Query API** | `GET /logs?campaignId=` → full delivery history |
| **Success Criteria** | JWT‑secured, idempotent, retries demonstrable by logs/tests, Postman docs |

---

## 💡 Section B – AI Brief Generator
**Objective**  
Generate structured influencer briefs via an LLM and internal “tools”.

| Area | Spec |
|------|------|
| **Endpoint** | `POST /generate‑brief` `{ brand, product, goal, platform }` |
| **Tools (Functions)** | `trendFetcher()` → 3 hashtags • `personaClassifier()` • `creativeAngle()` |
| **LLM Orchestration** | OpenAI / Ollama / HF + LangChain or function‑calling; output JSON brief (caption, hookIdeas, hashtags, CTA, tone) |
| **Caching** | Redis keyed by request hash; sensible TTL |
| **Resilience** | Retry LLM timeouts; only final failure returns 5xx |
| **Success Criteria** | Modular prompt templates, cache <100 ms on repeat, unit test for tool‑chain |

---

## 📊 Section C – Streaming Metrics Pipeline
**Objective**  
Ingest live engagement events and expose rolling analytics.

| Area | Spec |
|------|------|
| **Ingestion** | Consume JSON events from Kafka / Redis Streams / PubSub |
| **Computation** | Maintain stats per campaign: last 1 h • last 24 h • cumulative |
| **Storage** | Time‑series DB (Timescale) or bucketed SQL tables |
| **Query API** | `GET /campaigns/:id/metrics` → `{ window1h, window24h, total }` |
| **Latency Target** | ≤ 300 ms cold response |
| **Success Criteria** | Consumer processes sample stream, README explains windowing, benchmark proves latency |

---

## 📈 Section D – Batch ETL & Top‑Performer API
**Objective**  
Crunch a 10 k‑row CSV and surface highest‑engagement creators.

| Area | Spec |
|------|------|
| **ETL Script** | Chunk‑read `performance.csv`; compute `engRate = (likes+comments+shares)/views*100` |
| **Storage** | `campaignId, influencerId, engagementRate` |
| **API** | `GET /top‑performers?campaignId=&limit=` • sorted desc |
| **Runtime Budget** | < 2 min on 10 k rows |
| **Success Criteria** | Skips malformed rows, make‑style ETL command, unit test for rate + sorting |

---

### 🌍 Global Expectations
1. **Pick any two sections** and go deep.  
2. Docker / `make` spin‑up in ≤ 3 steps.  
3. No secrets in repo — use `.env.example`.  
4. Clear commit history + docs.  
5. Bonus extras (deployment, RAG, observability) are nice‑to‑haves, not required.

---

## 🌟 Bonus (Optional)

These are *not* expected — just fun ways to go the extra mile:

- 🌍 Deploy on **GCP / Cloud Run / Render** and share live endpoints  
- 🧠 Add **RAG/vector search** for personalized AI brief generation  
- 🔔 Build a **Slack/Telegram bot** for campaign status updates  
- 📈 Add observability with **logs, metrics, or Grafana dashboards**

---

## 🛠 Tech Stack Guidelines

Use what you're best at, but here’s what we love:

- **Languages:** TypeScript (Node.js), Python (FastAPI)
- **Database:** PostgreSQL, MongoDB, Redis, SQLite (for quick setup)
- **Queues/Streams:** Kafka, RabbitMQ, Redis Streams, GCP Pub/Sub
- **AI/LLMs:** OpenAI, HuggingFace, Ollama, LangChain, Function Calling APIs

---

## 🚀 Deliverables

- ✅ A GitHub repo (this one or forked)
- ✅ A clean and thoughtful `README.md` (that’s this file!)
- ✅ API docs — Swagger, Postman collection, or cURL samples
- ✅ Clear folder structure, meaningful commits
- ✅ (Optional) Loom video (3–5 mins) walking through your architecture and decisions

---

## 📄 README Must Include:

- Which 2 tracks you chose and why
- How to run your project locally (`docker-compose up` preferred)
- What trade-offs, assumptions, and decisions you made
- Instructions for testing or simulating API calls

---

## 🧠 How You’ll Be Evaluated

| Criteria                        | Weight |
|--------------------------------|--------|
| Architecture & Design Thinking | 25%    |
| Code Quality & Modularity      | 20%    |
| Correctness & Error Handling   | 20%    |
| AI/LLM Prompt + Tool Use       | 20%    |
| Documentation & DX             | 15%    |
| Bonus (If any)                 | 🌟     |

We’re most interested in your **thought process** — comments, docs, even "here's what I'd do next" notes help us see how you think.

---

## 🕐 Timeline & Submission

- **Time to complete:** 5–7 days from the date of receipt
- **Submit via email:** careers@famekeeda.com  
- **Subject line:** `Backend Challenge – <Your Name>`  
- **Repo visibility:** Public or invite us to your private repo

We respond to every candidate within 7 days of submission. Standout entries get a deep-dive call with our tech team.

---

## ✅ Tips for Success

- Scope consciously. **Nail 2 sections > rush 4**
- Don’t overengineer — make it clean, not complex
- Write secure, production-grade endpoints (e.g., auth, retries, caching)
- Real APIs are welcome, but good mocks are fine
- `.env.example` instead of hardcoding secrets

---

## 👋 Final Note

This is not a “test.” It’s your chance to **build something real**, just like we do every week. If you love blending AI with scalable backend systems, you’ll love this role.

We can’t wait to see what you create.

— The Fame Keeda R&D Team 💫
