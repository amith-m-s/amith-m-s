<div align="center">

```
╔═══════════════════════════════════════════════════════════════════════════╗
║                                                                           ║
║     █████╗ ███╗   ███╗██╗████████╗██╗  ██╗    ███╗   ███╗███████╗       ║
║    ██╔══██╗████╗ ████║██║╚══██╔══╝██║  ██║    ████╗ ████║██╔════╝       ║
║    ███████║██╔████╔██║██║   ██║   ███████║    ██╔████╔██║███████╗       ║
║    ██╔══██║██║╚██╔╝██║██║   ██║   ██╔══██║    ██║╚██╔╝██║╚════██║       ║
║    ██║  ██║██║ ╚═╝ ██║██║   ██║   ██║  ██║    ██║ ╚═╝ ██║███████║       ║
║    ╚═╝  ╚═╝╚═╝     ╚═╝╚═╝   ╚═╝   ╚═╝  ╚═╝    ╚═╝     ╚═╝╚══════╝       ║
║                                                                           ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

**Backend Engineer · NLP Systems · Cloud Infrastructure · Blockchain**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/amith-m-s/)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://portfolio-beryl-five-zezv4gffmv.vercel.app/)
[![Live Demo](https://img.shields.io/badge/Deep_Resume_Analyzer-live-22c55e?style=flat-square&logo=vercel&logoColor=white)](https://deep-resume-analyzer.vercel.app)
[![Email](https://img.shields.io/badge/amith6567@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:amith6567@gmail.com)
[![Views](https://komarev.com/ghpvc/?username=amith-m-s&style=flat-square&color=475569&label=profile+views)](https://github.com/amith-m-s)

</div>

---

## The Work

Pre-final year engineer at Rajagiri School of Engineering & Technology. I build backend systems with real infrastructure — multi-tenant platforms, async pipelines, cloud-native deployments, NLP services, and on-chain contracts. Everything listed here is code that exists, compiles, and runs.

| Project | What it actually is | Core Tech | Status |
|---|---|---|---|
| [Cloud God Platform](https://github.com/amith-m-s/aws) | Production AWS platform — FastAPI + PostgreSQL + Redis + S3 + SQS + RAG + Terraform IaC | FastAPI · PostgreSQL · Redis · SQS · S3 · Terraform · Docker | [![IaC](https://img.shields.io/badge/terraform-ready-7B42BC?style=flat-square&logo=terraform&logoColor=white)](https://github.com/amith-m-s/aws) |
| [RelayForge](https://github.com/amith-m-s/RelayForge) | Multi-tenant webhook delivery & API automation platform — HMAC signing, Celery workers, Alembic migrations, idempotency | FastAPI · PostgreSQL · Redis · Celery · SQLAlchemy 2.x · Alembic | [![Repo](https://img.shields.io/badge/production--scaffold-475569?style=flat-square)](https://github.com/amith-m-s/RelayForge) |
| [Deep Resume Analyzer](https://github.com/amith-m-s/Deep-Resume-Analyzer) | Semantic resume-JD matching with hybrid ATS scoring — 3-service Docker system | FastAPI · Express.js · React · Docker · NLP Embeddings | [![Live](https://img.shields.io/badge/live-22c55e?style=flat-square)](https://deep-resume-analyzer.vercel.app) |
| [LootBox Game](https://github.com/amith-m-s/lootbox-game) | Fully on-chain NFT loot boxes — provably fair randomness, AdminCap pattern, pity guarantee | Sui Move · Smart Contracts | [![On-chain](https://img.shields.io/badge/on--chain-4DA2FF?style=flat-square)](https://github.com/amith-m-s/lootbox-game) |
| [JARVIS](https://github.com/amith-m-s/JARVIS) | Offline voice assistant — NLP intent classification, self-registering plugin architecture | Python · NLP · Speech Recognition | [![Repo](https://img.shields.io/badge/repo-475569?style=flat-square)](https://github.com/amith-m-s/JARVIS) |
| [Pro Finance Tracker](https://github.com/amith-m-s/Pro-Finance-Tracker) | Zero-backend finance dashboard — Chart.js, LocalStorage, 10+ categories, budget alerts | Vanilla JS · Chart.js · LocalStorage | [![Repo](https://img.shields.io/badge/repo-475569?style=flat-square)](https://github.com/amith-m-s/Pro-Finance-Tracker) |

---

## Cloud God Platform — Production AWS Architecture

> FastAPI · PostgreSQL + pgvector · Redis · SQS · S3 · ECS Fargate · Terraform

The most complete system in this portfolio. Full cloud-native stack with infrastructure as code.

```
Client
  │
  ▼
CloudFront  ──────────────────────────────────────────────────────┐
  │                                                               │
  ▼                                                         (static assets)
Application Load Balancer (ALB)                                   S3
  │
  ▼
FastAPI Service  (ECS Fargate)
  ├── /api/v1/auth     JWT auth · bcrypt · refresh token rotation
  ├── /api/v1/docs     Upload · chunk · embed · index per tenant
  ├── /api/v1/chat     RAG — semantic search → context → answer
  ├── /health          liveness probe
  ├── /ready           readiness probe  (DB + Redis checks)
  └── /metrics         Prometheus export
       │              │                │
       ▼              ▼                ▼
  PostgreSQL       Redis           SQS Queue
  + pgvector      (cache +        (async doc
  (vectors,        idempotency     pipeline)
  tenants,         + rate              │
  docs)            limiting)          ▼
                               Worker (ECS Fargate)
                               chunk → embed → index → S3
```

**What makes this production-grade:**
- Terraform IaC: VPC, subnets, NAT Gateway, ALB, ECS Fargate with auto-scaling, S3 with AES-256 + versioning, SQS with DLQ (maxReceiveCount=3), CloudWatch log groups with 14-day retention, IAM least-privilege roles
- Observability: structured JSON logging, Prometheus metrics, per-request trace IDs via middleware
- Security: CORS hardening, input validation, bcrypt password hashing, rate limiting at middleware layer, `.env.example` secrets pattern

---

## RelayForge — Multi-Tenant Webhook Platform

> FastAPI · PostgreSQL · Redis · Celery · SQLAlchemy 2.x · Alembic · GitHub Actions CI

Webhook delivery infrastructure — the kind of system that powers Stripe, GitHub, and Segment's event pipelines.

```
Inbound Event
      │
      ▼
API Layer  (FastAPI)
  ├── Multi-tenant org + membership model
  ├── JWT access tokens + refresh-token rotation
  ├── API key authentication
  └── Request idempotency (Redis-backed)
      │
      ▼
Service Layer
  ├── Event intake + validation
  ├── HMAC webhook signing  ← tamper-proof delivery
  ├── Delivery orchestration
  └── Attempt tracking + retry scheduling
      │
      ▼
Worker Layer  (Celery + Redis)
  ├── celery beat for scheduled retries
  └── Delivery attempt persistence → PostgreSQL
```

**The engineering decisions that matter:**
- SQLAlchemy 2.x with Alembic migrations — schema changes are versioned and reversible
- HMAC signing on every webhook — consumers can verify payload integrity without a shared secret in the URL
- Redis idempotency keys — duplicate event intake is deduplicated at the middleware layer before hitting business logic
- Repository pattern separating persistence from service logic — testable without a live database

---

## Deep Resume Analyzer — Semantic NLP System

> React · Express.js · FastAPI · Docker Compose · NLP Embeddings · Vercel

```
┌─────────────┐     HTTP/REST      ┌──────────────────┐     REST      ┌────────────────────┐
│  React      │ ─────────────────▶ │  Express.js      │ ────────────▶ │  FastAPI           │
│  Frontend   │                    │  API Gateway     │               │  NLP Microservice  │
│  Vercel CDN │ ◀───────────────── │  JWT · RateLimit │ ◀──────────── │  Embedding Engine  │
└─────────────┘                    └──────────────────┘               └────────────────────┘
                                                                              │
                                                               normalize → tokenize → embed
                                                               → score → rank → gap-detect
```

**The scoring model:**
```
final_score = (0.60 × cosine_similarity)   ← semantic context
            + (0.25 × keyword_precision)   ← exact term matching
            + (0.15 × domain_skill_score)  ← validated skill overlap
```
Cosine similarity alone over-weights semantic context. A data science resume against a frontend JD scores 70%+ on semantics. The hybrid model eliminates that. Each pipeline stage has clean I/O interfaces — the embedding layer is swappable for BERT or a fine-tuned transformer without touching scoring logic.

---

## LootBox Game — On-chain Contract

> Sui Move · On-chain Randomness · NFT Lifecycle · AdminCap Pattern

```
❌  public fun open_loot_box(r: &Random, ...)
    ↳ Caller simulates outcome → aborts if unfavorable → front-running

✅  entry fun open_loot_box(r: &Random, ...)
    ↳ RandomGenerator consumed inside entry fn → simulation impossible
```

`sui::random` must be consumed inside an `entry` function — this is a Sui-specific constraint most Move contracts get wrong. The pity system tracks consecutive non-Legendary opens per wallet via `Table<address, u8>` dynamic fields. Rarity: Common 60% · Rare 25% · Epic 12% · Legendary 3%.

---

## Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript_ES6+-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![Sui Move](https://img.shields.io/badge/Sui_Move-4DA2FF?style=flat-square&logo=sui&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?style=flat-square&logo=csharp&logoColor=white)
![C++](https://img.shields.io/badge/C++-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express.js-000000?style=flat-square&logo=express&logoColor=white)
![Celery](https://img.shields.io/badge/Celery-37814A?style=flat-square&logo=celery&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat-square&logo=terraform&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat-square&logo=amazonaws&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)

---

## Engineering Principles — Evidence, Not Claims

| Principle | Where it shows up |
|---|---|
| **Infrastructure as code** | Cloud God Platform — complete Terraform stack: VPC, ECS, ALB, S3, SQS, IAM, CloudWatch |
| **Schema migrations are versioned** | RelayForge — Alembic with `alembic/` directory; production schema changes are reversible |
| **Idempotency is a first-class concern** | RelayForge — Redis-backed idempotency keys deduplicate at middleware before business logic |
| **Observability before features** | Cloud God Platform — structured JSON logging, Prometheus `/metrics`, request ID tracing |
| **Security by architecture** | RelayForge HMAC signing · Cloud God JWT + bcrypt · LootBox AdminCap pattern |
| **Modular pipeline = swappable layers** | Deep Resume Analyzer — embedding layer isolated; BERT integration requires zero scoring rewrites |

---

## What I'm Working On

- Deep Resume Analyzer v2 — replacing sentence embeddings with fine-tuned BERT; pipeline is already staged for this
- RelayForge deeper domain implementation — delivery attempt retry logic, DLQ handling, tenant billing hooks
- LeetCode daily grind — trees, DP, graphs; building the DSA depth that interviews actually test
- PostgreSQL internals — query planning, VACUUM mechanics, index strategies (not listing it as a skill until I can explain an `EXPLAIN ANALYZE` output)

---

## Education & Credentials

```
B.Tech — Computer Science & Business Systems         2023 – 2027
Rajagiri School of Engineering & Technology · KTU
CGPA: 7.90 / 10.0

Microsoft Certified: Foundational C# ........... 91.3%
HackerRank Certified Software Engineer
NPTEL: Database Management Systems (IIT Kharagpur)
Fortinet Certified Associate in Cybersecurity
Python Intern — Futura Labs, Calicut ........... Jun 2025
```

---

## Currently Seeking

Backend engineering or cloud infrastructure internships at product companies building real systems — not demo apps.

**[amith6567@gmail.com](mailto:amith6567@gmail.com)** · **[amithms.dev](https://portfolio-beryl-five-zezv4gffmv.vercel.app/)** · **[linkedin.com/in/amith-m-s](https://www.linkedin.com/in/amith-m-s/)**

---

<div align="center">
<sub>Every claim on this page is backed by code that exists, compiles, and runs.</sub>
</div>
