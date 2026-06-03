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

**Backend Engineer · NLP Systems · Blockchain / Web3**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/amith-m-s/)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://amithms.dev)
[![Live Demo](https://img.shields.io/badge/Deep_Resume_Analyzer-Live-22c55e?style=flat-square&logo=vercel&logoColor=white)](https://deep-resume-analyzer.vercel.app)
[![Email](https://img.shields.io/badge/amith6567@gmail.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:amith6567@gmail.com)
[![Views](https://komarev.com/ghpvc/?username=amith-m-s&style=flat-square&color=475569&label=profile+views)](https://github.com/amith-m-s)

</div>

---

## The Work

I'm a pre-final year engineer at Rajagiri School of Engineering & Technology building systems that are deployed, observable, and architecturally honest.

Three things I've actually shipped:

| Project | What it does | Stack | Status |
|---|---|---|---|
| [Deep Resume Analyzer](https://github.com/amith-m-s/Deep-Resume-Analyzer) | Semantic resume-JD matching with hybrid ATS scoring | FastAPI · Express · React · Docker · NLP Embeddings | [![Live](https://img.shields.io/badge/live-22c55e?style=flat-square)](https://deep-resume-analyzer.vercel.app) |
| [LootBox Game](https://github.com/amith-m-s/lootbox-game) | On-chain NFT loot boxes with provably fair randomness | Sui Move · Smart Contracts | [![Repo](https://img.shields.io/badge/repo-475569?style=flat-square)](https://github.com/amith-m-s/lootbox-game) |
| [JARVIS](https://github.com/amith-m-s/JARVIS) | Offline voice assistant with plugin-based command routing | Python · NLP · Speech Recognition | [![Repo](https://img.shields.io/badge/repo-475569?style=flat-square)](https://github.com/amith-m-s/JARVIS) |

---

## Deep Resume Analyzer — Technical Breakdown

> 3-service Docker system. Deployed on Vercel. End-to-end PDF processing under 3 seconds.

```
┌─────────────┐     HTTP/REST     ┌──────────────────┐     REST      ┌─────────────────────┐
│   React     │ ────────────────▶ │  Express.js      │ ────────────▶ │   FastAPI           │
│   Frontend  │                   │  API Gateway     │               │   NLP Microservice  │
│   Vercel CDN│ ◀──────────────── │  JWT · Rate Limit│ ◀──────────── │   Embedding Engine  │
└─────────────┘                   └──────────────────┘               └─────────────────────┘
                                                                               │
                                                                    normalize → tokenize
                                                                    → embed → score
                                                                    → rank → gap-detect
```

**The scoring model I designed:**

```
final_score = (0.60 × cosine_similarity)   ← semantic context
            + (0.25 × keyword_precision)   ← exact term matching
            + (0.15 × domain_skill_score)  ← validated skill overlap
```

Cosine similarity alone over-weights semantic context — a data science resume against a frontend JD can score 70%+ on semantics alone. The hybrid model penalizes that. Each weight was chosen to reflect how real recruiters actually triage: context matters most, but missing exact keywords still fails ATS, and skills are a binary gate.

The pipeline stages have clean interfaces — the embedding layer is swappable. BERT or a fine-tuned transformer plugs in without touching the scoring or ranking logic.

---

## LootBox Game — On-chain Design Decisions

> Sui Move. Every mechanic lives on-chain. No oracle. No off-chain state.

The non-obvious engineering here is the randomness architecture:

```
❌  public fun open_loot_box(r: &Random, ...)
    ↳ Caller can simulate outcome, abort if unfavorable → front-running

✅  entry fun open_loot_box(r: &Random, ...)
    ↳ RandomGenerator consumed inside entry fn → simulation impossible
```

`sui::random` must be consumed inside an `entry` function — making it `public` hands callers the ability to inspect outcomes before committing. Most Move tutorials skip this. Most contracts get it wrong.

```
Rarity model:   Common 60% · Rare 25% · Epic 12% · Legendary 3%
Pity system:    Table<address, u8> dynamic fields
                → guaranteed Legendary after 30 consecutive non-Legendary opens
Access control: AdminCap pattern → treasury + rarity updates gated behind capability object
Payment:        Exact Coin<SUI> enforcement → partial payments abort at the type level
```

---

## JARVIS — Architecture Principle

```
Speech Input
     │
     ▼
Offline STT  (no network dependency)
     │
     ▼
Intent Classifier  (NLP — keyword + context matching)
     │
     ▼
Plugin Router  ←── plugins self-register here
     │                router has zero compile-time imports of plugins
     ▼
Executor  [file ops · web · system · custom]
```

The router never imports a specific plugin at compile time. Plugins register themselves on load. New commands = new file dropped in `/plugins`. Zero modification to routing logic. Same pattern as Flask blueprints, Express routers, or any well-designed plugin host.

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
![React](https://img.shields.io/badge/React-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![Tailwind](https://img.shields.io/badge/Tailwind-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)

![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)

---

## What I'm Actively Working On

- **Deep Resume Analyzer v2** — replacing the current sentence embedding model with a fine-tuned BERT transformer; pipeline architecture is already staged to support this without rewriting scoring logic
- **LeetCode DSA grind** — daily problems, focus on trees, DP, and graph traversal; building the foundations that FAANG screens actually test
- **PostgreSQL depth** — learning query planning, index strategy, and VACUUM mechanics; not listing it as a skill until I can explain a query plan

---

## By the Numbers

```
1    production deployment (live, not just localhost)
1    on-chain contract (deployed to Sui testnet)
3    containerized services (Docker Compose, environment parity)
91%  Microsoft C# certification score
7.90 CGPA at Rajagiri School of Engineering & Technology
```

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
# 📊 GitHub Analytics

<p align="center">

  <img width="70%" src="https://github-readme-streak-stats-eight.vercel.app/?user=amith-m-s&theme=tokyonight&hide_border=true"/>

</p>

---

## Currently Seeking

Backend engineering or AI infrastructure internships at product companies building real systems.

**[amith6567@gmail.com](mailto:amith6567@gmail.com)** · **[amithms.dev](https://amithms.dev)** · **[linkedin.com/in/amith-m-s](https://www.linkedin.com/in/amith-m-s/)**

---

<div align="center">
<sub>Every claim on this page is backed by code that exists and runs.</sub>
</div>
