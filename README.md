<!-- ═══════════════════════════════════════════════════════════════════════════
     AMITH M S — GitHub Profile README
     Designed to survive technical scrutiny. Every claim is backed by evidence.
     ═══════════════════════════════════════════════════════════════════════════ -->

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
║              Backend Engineer  ·  NLP Systems  ·  Blockchain             ║
╚═══════════════════════════════════════════════════════════════════════════╝
```

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/amith-m-s/)
[![Portfolio](https://img.shields.io/badge/Portfolio-000000?style=flat-square&logo=vercel&logoColor=white)](https://amithms.dev)
[![Email](https://img.shields.io/badge/Email-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:amith6567@gmail.com)
[![Profile Views](https://komarev.com/ghpvc/?username=amith-m-s&style=flat-square&color=0F2027)](https://github.com/amith-m-s)

</div>

---

## Who I Am

I'm a backend-focused engineer in my pre-final year at Rajagiri School of Engineering & Technology (CSBS, KTU). I ship systems end-to-end — from NLP microservices in Docker to on-chain smart contracts in Sui Move.

I'm not trying to look senior. I'm trying to **build things that are real** — deployed, functional, technically honest work.

> "I design for the next order of magnitude before optimizing prematurely."

---

## What I've Shipped

### Deep Resume Analyzer — *Production · [Live Demo](https://deep-resume-analyzer.vercel.app)*

> A semantic resume-to-JD matching platform. Not a tutorial. Not a clone. A real system with a custom scoring model.

```
Architecture:  React  →  Express.js Gateway  →  FastAPI NLP Service
Infra:         Docker Compose · Vercel CDN · environment parity dev↔prod
Core model:    final_score = 0.60 × cosine_similarity
                           + 0.25 × keyword_precision
                           + 0.15 × domain_skill_score
Pipeline:      normalize → tokenize → embed → score → rank → gap-detect
Throughput:    end-to-end PDF processing < 3s
```

**Why the weights?** Cosine similarity alone over-weights context at the expense of exact terminology. The hybrid model reduces false-positive candidate rankings — a semantic match on a data science resume against a frontend JD no longer scores 70%.

The NLP pipeline is staged with clean interfaces between layers — swapping in BERT or a fine-tuned transformer doesn't require rewriting scoring logic.

---

### LootBox Game — *On-chain · [GitHub](https://github.com/amith-m-s/lootbox-game)*

> A fully on-chain NFT loot box in Sui Move. Every game mechanic lives on-chain. No trusted randomness oracle — just the protocol.

```
Design decisions:
  - RandomGenerator consumed inside entry fn (non-public) → front-running impossible
  - Exact Coin<SUI> enforcement → no partial payments
  - AdminCap pattern → treasury/rarity gated behind capability object
  - Pity counter via Table<address, u8> dynamic fields → guaranteed Legendary after 30 opens

Rarity distribution:
  Common 60%  ·  Rare 25%  ·  Epic 12%  ·  Legendary 3%
```

**The hard part:** `sui::random` can only be consumed inside an `entry` function. Making it `public` breaks the randomness guarantee — a caller could simulate outcomes and selectively abort. This is a Sui-specific constraint most Move tutorials skip. I didn't.

---

### JARVIS — *Offline Voice Assistant · [GitHub](https://github.com/amith-m-s/JARVIS)*

> Python voice assistant with NLP intent classification and a plugin-based command architecture. New commands extend the system without touching routing logic.

```
Input → STT (offline) → intent classifier → plugin router → executor
                                              ↓
                                   [file ops | web | system | custom]
```

Core principle: the router never imports a specific plugin. Plugins register themselves. This is the same pattern as Flask blueprints or Express routers — zero-downtime feature additions.

---

### Pro Finance Tracker — *Client-side · [GitHub](https://github.com/amith-m-s/Pro-Finance-Tracker)*

> Zero-backend personal finance dashboard. LocalStorage as the persistence layer. Chart.js for real-time category visualizations. No server, no cost, no privacy leak.

```
10+ expense categories  ·  overspend alerts  ·  monthly trend analytics
Pure Vanilla JS — no framework tax for a tool this focused
```

---

## Technical Stack

```
Backend        Node.js · Express.js · FastAPI · REST APIs · JWT Auth · Rate Limiting
AI / NLP       Sentence Embeddings · Cosine Similarity · NLP Pipelines · Skill-Gap Detection
Blockchain     Sui Move · Smart Contracts · NFT Lifecycle · On-chain Randomness · Sui CLI
Frontend       React.js · Tailwind CSS · Chart.js · HTML5/CSS3
Databases      MySQL · SQLite · Data Modeling · Schema Design · Indexing
DevOps         Docker · Docker Compose · GitHub Actions · Vercel · Linux · CI/CD
Languages      Python · JavaScript (ES6+) · SQL · Sui Move · C# · C/C++
CS Core        DSA · OOP · DBMS · OS · Computer Networks · System Design
```

---

## Engineering Principles I Actually Follow

These aren't aspirational buzzwords. Here's what they mean in my work:

| Principle | What it looks like in my code |
|---|---|
| **Contracts before code** | NLP pipeline has defined input/output interfaces between stages — swap the embedding layer without touching scoring |
| **Design for the next order of magnitude** | FastAPI NLP service is isolated so it can be horizontally scaled independently of the Express gateway |
| **Security is not an afterthought** | AdminCap pattern in Move — treasury ops fail if capability object is absent, not just if caller is wrong address |
| **Modularity means zero coupling** | JARVIS plugin system: router has no import of any plugin at compile time |

---

## What I'm Working On

- Integrating BERT-based sentence transformers into the Deep Resume Analyzer pipeline (replacing the current lighter embedding model)
- Exploring distributed messaging patterns (Redis Pub/Sub, queue-based decoupling)
- LeetCode grind — daily commitment to DSA depth (arrays, trees, DP)
- Learning PostgreSQL properly, not "learning" in the resume-skills sense

---

## Education & Credentials

```
B.Tech — Computer Science & Business Systems (CSBS)       2023–2027
Rajagiri School of Engineering & Technology · KTU
CGPA: 7.90 / 10.0  ·  Currently Semester 5

Microsoft Certified: Foundational C# — Score: 91.3%
HackerRank Certified Software Engineer
NPTEL: Database Management Systems — IIT Kharagpur
Fortinet Certified Associate in Cybersecurity
AWS Machine Learning Learning Path (in progress)
```

---

## GitHub Stats

<div align="center">
  <img height="160" src="https://github-readme-stats.vercel.app/api?username=amith-m-s&show_icons=true&theme=github_dark&hide_border=true&count_private=true"/>
  <img height="160" src="https://github-readme-stats.vercel.app/api/top-langs/?username=amith-m-s&layout=compact&theme=github_dark&hide_border=true"/>
</div>

---

## Currently Seeking

Backend engineering or AI infrastructure internships at product companies that care about system design quality — not just feature velocity.

If you've read this far and something looks interesting, reach me at **[amith6567@gmail.com](mailto:amith6567@gmail.com)**.

---

<div align="center">
<sub>Built by someone who reads the internals, not just the docs.</sub>
</div>
