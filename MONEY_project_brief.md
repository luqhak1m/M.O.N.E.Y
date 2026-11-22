# M.O.N.E.Y. — Project Brief

Personal expense tracker. Paste this into a new session so the assistant has the full picture and can guide me.

## About me
- Luqman, junior backend engineer in Malaysia (Shah Alam), about 11 months into my first job at DIMEX Sdn. Bhd. (January 2026 – present).
- Built backend features on two AI products: InsightChat (Django/DRF, MySQL, Celery, Redis, Kubernetes/ArgoCD on DigitalOcean) and HWG AI (FastAPI, Tortoise ORM, PostgreSQL, Redis, APScheduler, Docker on Alibaba Cloud). This included Stripe payments with row-locked, idempotent crediting, a LangChain agent pipeline with a booking agent, OAuth integrations for Meta/TikTok/Shopify, and a nightly analytics sync.
- Side project: AidVise, an AI career guidance platform (FastAPI, PostgreSQL, Celery, Flutter). It's private because it's an ongoing business pitch.
- Goal: become a backend engineer in **fintech**, and long term a software architect. I'm moving towards **Java** because I like enforced types and OOP. I studied OOP and SOLID at university.
- Most of my work code was written with AI assistance. I want to rebuild my hands-on coding skill.

## What M.O.N.E.Y. is
- Named as a homage to the song "M.O.N.E.Y." by The 1975. Use the title only: no lyrics or artwork.
- Replaces my daily Excel expense tracker. I enter **date, title, amount, payment method and payment category**. The remaining money and the budgets are calculated automatically.
- Version 1 copies the Excel mechanism faithfully in code. The Excel file is the source of truth for expected results.
- It will be live on my own domain and in a public GitHub repo. It's not promoted and has one user: me.

## Why I'm building it
1. **Practise low-level coding by hand.** Rule: **AI does not write this project's code.** AI may review my code, explain concepts and point me to docs, but I write the code.
2. **A first app to set up my VPS and deploy pipeline**, which later projects will reuse.
3. **A fintech talking point**: designed as a proper ledger, not a basic CRUD app.

## Design principles
- **Money is stored as integers in sen** (`long`), never `float` or `double`.
- **An append-only ledger:** each transaction is an immutable record. Corrections are new entries, not edits.
- **Balances and budgets are derived** from the transaction list, never stored as a running total that can drift.
- **A pure calculation core:** money logic (balances, budgets, category totals) lives in plain Java classes with no web or database code. The web layer only calls it.
- **Unit-test every calculation against my Excel numbers.**
- **Import my existing Excel data** (CSV import).
- It holds real personal financial data, so it needs a login, HTTPS and tested backups.

## Stack (decided)
- **Backend:** Java with **Javalin** (thin web framework) and **plain JDBC** (hand-written SQL and transactions). I chose Javalin over Spring Boot on purpose, to learn what frameworks normally hide.
- **Database:** PostgreSQL, with **Flyway** migrations.
- **Tests:** JUnit 5.
- **Build:** Gradle or Maven. Pick one and stick with it.
- **Frontend:** Vue 3 as a **PWA**, installable on my phone, so there's no separate mobile app.
- **Deploy:** Docker image, GitHub Actions, then my VPS.

## VPS and infrastructure (shared with my other projects)
- **Hostinger VPS** (KVM 1 or KVM 2) running Ubuntu with Docker.
- **Docker Compose** per app: app container plus PostgreSQL container, with data in Docker volumes.
- **Caddy** reverse proxy: automatic HTTPS, routing by subdomain (e.g. `money.mydomain.com`).
- **Security:**
  - firewall (Hostinger's plus `ufw`) allowing only 80, 443 and SSH
  - SSH key-only, no root or password login, `fail2ban`
  - automatic security updates
  - **database and Redis never exposed to the internet** (internal Docker network only)
  - Cloudflare DNS in front (optional proxy)
- **Backups:** nightly `pg_dump`, sent **off the server** to Cloudflare R2 or Backblaze B2. Restore tested at least once.
- **Deploy pipeline I write myself:** GitHub Actions builds the image, pushes it to GitHub Container Registry, then SSHes in and runs `docker compose pull && docker compose up -d`.
- **Secrets** in a `.env` on the server, never committed.
- A bucket isn't needed for this app.

## Plan (aim: live in 2–3 weekends)
1. Recreate the Excel logic as a pure Java calculation core with JUnit tests matched to the Excel results.
2. PostgreSQL schema plus Flyway migrations, a JDBC data layer and CSV import of my Excel history.
3. Javalin REST API plus auth.
4. Vue 3 PWA frontend.
5. VPS setup, Docker Compose, Caddy, backups and the GitHub Actions deploy, then go live.
6. Optional later: recurring expenses, monthly reports, budget alerts.

## How I want to be guided
- Explain concepts and trade-offs, review my code and suggest what to look up. **Don't write the implementation for me** unless I explicitly ask.
- Plain, simple language, but technical terms are fine.
- Be honest and critical, and flag bad practice.
- Keep me focused on finishing. Scope creep is my weakness.

## After M.O.N.E.Y.
The next project is a **music streaming service simulator** in Java and Spring Boot (separate brief). It will deploy onto the same VPS and pipeline.
