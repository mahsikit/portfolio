# CLAUDE.md — portfolio-website

## Owner

**Jonathan Santoso** — Analytics Engineer / Senior Data Analyst
`jonathan.santoso3910@gmail.com` · linkedin.com/in/jonathansantoso

---

## What This Is

A personal portfolio website to replace the existing Wix site (`joso39.wixsite.com/portofolio`).
Deployed publicly on **Vercel**. The site showcases 8 real production projects (anonymized), work
experience, skills, certifications, and a Springer publication.

---

## Tech Stack Decision

### Framework: **Astro v4**
- Static site generator — ships zero JavaScript by default (fast, great Lighthouse score)
- Component-based like React but without the hydration overhead
- Built-in Markdown/MDX support — project content can be written as `.md` files
- Vercel deploys Astro with zero config
- Better than Next.js for a portfolio: no server needed, no over-engineering

### Styling: **Tailwind CSS v3**
- Utility-first, fast to build with
- Pairs perfectly with Astro
- No separate CSS files to maintain

### Deployment: **Vercel**
- Free tier, custom domain support
- Auto-deploy on `git push` to `main`
- Connect the GitHub repo → Vercel detects Astro automatically

### No CMS, no database — all content is static (hardcoded or `.md` files).

---

## Design Direction

### Aesthetic: **Clean dark dashboard**

Jonathan's entire work life is spent in BI dashboards (Tableau, Power BI, Metabase). The portfolio
should feel like a well-designed analytics product — not a generic dev portfolio, not a terminal
emulator, not overly flashy.

**The vibe:** professional, data-driven, minimal motion, high information density done cleanly.
Think: a dark Metabase or Grafana dashboard that a non-technical director would also trust.

### Color Palette

| Token | Value | Usage |
|---|---|---|
| Background | `#0f172a` (slate-900) | Page background |
| Surface | `#1e293b` (slate-800) | Cards, nav |
| Border | `#334155` (slate-700) | Card borders, dividers |
| Text primary | `#f1f5f9` (slate-100) | Headings, body |
| Text muted | `#94a3b8` (slate-400) | Captions, labels |
| Accent | `#38bdf8` (sky-400) | Links, highlights, badges, CTA hover |
| Accent subtle | `#0ea5e9` (sky-500) | Buttons, icons |
| Green | `#4ade80` (green-400) | Success states, "available" badge |

### Typography

- **Body / UI:** `Inter` (Google Fonts) — clean, professional, widely trusted
- **Code / tech tags:** `JetBrains Mono` — for skill badges, tech stack pills
- Scale: Tailwind defaults (`text-sm`, `text-base`, `text-lg`, `text-2xl`, `text-4xl`, `text-6xl`)

### Motion

Minimal. Only:
- Fade-in on scroll (Intersection Observer, no library needed)
- Subtle hover lift on project cards (`translate-y-[-2px]`, `shadow-lg`)
- Underline slide on nav links
- No typing animations, no parallax, no particle effects

---

## Site Structure

Single-page with smooth-scroll sections + a sticky top navbar. No separate pages needed.

**Section order rationale:** Experience leads the body because Jonathan has real corporate
credentials (Senior title, Freeport Indonesia, Board-level deliverables). Projects support
the experience — they prove *how* he works technically, not the headline. The stat bar in
the hero communicates scale instantly before anyone reads a word.

```
/
├── #hero         — Name, title, stat bar (4 numbers), CTA buttons
├── #experience   — 2-item work timeline  ← comes BEFORE projects
├── #projects     — 8 project cards (grid)
├── #skills       — Grouped skill tags
├── #certifications — 5 cert badges + publication card
└── #contact      — Email + social links
```

### Navbar

Sticky, dark, glass-blur effect (`backdrop-blur-sm bg-slate-900/80`).
Links: About · Projects · Experience · Skills · Contact
Right side: GitHub icon · LinkedIn icon · "Download CV" button (links to hosted PDF).

---

## Section Specs

### Hero

```
[Available for opportunities] ← small green badge, top-left

Jonathan Santoso
Analytics Engineer  ·  Senior Data Analyst    ← both titles

┌────────┬────────┬────────┬────────┐
│  2+    │   8    │  3.89  │   1    │
│  yrs   │ tools  │  GPA   │ paper  │
│        │ built  │        │Springer│
└────────┴────────┴────────┴────────┘

[View Experience ↓]   [Download CV]   [GitHub]   [LinkedIn]
```

- Stat bar sits directly below the name/title — no paragraph of text, just numbers
- Each stat box: `bg-slate-800 border border-slate-700 rounded-lg p-4 text-center`
- Stat number: `text-3xl font-bold text-sky-400`, label: `text-xs text-slate-400 mt-1`
- Full viewport height on desktop, auto on mobile
- Subtle dot-grid pattern background (CSS only — `radial-gradient` on `::before`)
- Accent line under the name (`border-b-2 border-sky-400 w-16`)
- No "About" paragraph section — the stat bar replaces it; bio context lives in Experience bullets

### Experience (comes before projects)

Vertical timeline, two items. This is the section that establishes credibility.

```
● Apr 2024 – Present
  Senior Data Analyst — an Indonesian insurtech, Jakarta
  · Built dbt warehouse replacing 4–6 hr/day Excel workflows → <10 min
  · Tableau dashboards presented to Board of Directors
  · Weekly insurance portfolio reviews for Board
  · dbt models: health, motor, marine, reinsurance lines

● Feb 2023 – Feb 2024
  Business Intelligence Analyst — a major Indonesian mining company, Jakarta
  · Led Teradata → Snowflake migration + ETL pipeline build
  · SAP BusinessObjects data mart for UOMS division
  · Power BI dashboards for dewatering operations team
  · Agile / daily scrum delivery
```

- Keep company names vague — LinkedIn has the real names for anyone who wants them
- Timeline dot: `w-3 h-3 rounded-full bg-sky-400`
- Left border line connecting dots: `border-l-2 border-slate-700`
- Date badge: `text-xs font-mono text-slate-400`

---

### Projects (supporting the experience, not the headline)

8 cards in a 3-column grid (2 on tablet, 1 on mobile).

Each card:
```
┌─────────────────────────────────────┐
│  [Python] [Streamlit] [Docker]      │  ← tech pill tags (top)
│                                     │
│  Pharma Price Optimizer             │  ← project title
│  One-sentence description           │
│                                     │
│  [GitHub →]                         │  ← links to public GitHub repo
└─────────────────────────────────────┘
```

Card hover: subtle border-sky-400 glow, 2px lift.

**Project data (hardcode these):**

| # | Title | Tags | GitHub slug |
|---|---|---|---|
| 1 | Pharma Price Optimizer | Python, Streamlit, Docker | pharma-price-optimizer |
| 2 | Motor Insurance Price Crawler | Python, GitHub Actions, openpyxl | motor-insurance-price-crawler |
| 3 | Health Claim Data Puller | Python, Playwright, Streamlit, Docker | health-claim-data-puller |
| 4 | User Export Automation | Python, Metabase API, Google Drive | user-export-automation |
| 5 | Claims Portal Scraper | Python, Streamlit, PostgreSQL, Docker | claims-portal-scraper |
| 6 | NAS → Cloud Sync | Python, SMB, Google Drive, Sheets | nas-cloud-sync |
| 7 | Claim Report Pipeline | Python, Gmail API, Google Drive | claim-report-pipeline |
| 8 | Monthly Report Tool | Python, Streamlit, Docker | monthly-report-tool |

> Replace `youruser` in GitHub links once the public GitHub username is confirmed.

### Skills

Grouped pill tags, 4 groups:

| Group | Skills |
|---|---|
| Languages & Query | SQL, Python, dbt |
| Cloud & Warehouse | Snowflake, BigQuery, GCP, AWS Redshift, Teradata, SQL Server |
| BI & Reporting | Tableau, Power BI, Metabase, SAP BusinessObjects |
| Engineering | Docker, Prefect, SAP Data Services, Data Modelling, Data Warehousing |

Each pill: `bg-slate-800 border border-slate-700 text-sky-300 font-mono text-xs rounded px-2 py-1`

### Certifications

5 cards in a row (wrap on mobile):
- Microsoft Certified: Power BI Data Analyst Associate
- Microsoft Certified: Azure Data Fundamentals
- Tableau Certified Data Analyst
- Python Essentials 1 — Cisco
- Foundations: Data, Data, Everywhere — Google

Each card: small icon (use devicons or emoji), cert name, issuer.

### Publication

Single card, wider:
```
📄 Sentiment Analysis on Google Play Store User Reviews
   of Digital Bank Applications in Indonesia
   Springer · March 2024
   [Read Paper →]
```

### Contact

Clean, centered:
```
Let's connect

jonathan.santoso3910@gmail.com

[LinkedIn]  [GitHub]  [Email]
```

---

## File Structure (Astro)

```
portfolio-website/
├── public/
│   ├── cv.pdf                    ← upload Jonathan_Santoso_Resume.pdf here
│   └── favicon.svg
├── src/
│   ├── layouts/
│   │   └── Layout.astro          ← base HTML shell, imports fonts, meta tags
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Hero.astro            ← name + stat bar + CTA buttons
│   │   ├── Experience.astro      ← timeline, comes FIRST in body
│   │   ├── ProjectCard.astro     ← accepts title, tags, desc, github props
│   │   ├── Projects.astro        ← renders grid of ProjectCards
│   │   ├── Skills.astro
│   │   ├── Certifications.astro  ← also includes Publication card
│   │   └── Contact.astro
│   ├── pages/
│   │   └── index.astro           ← assembles all sections
│   └── styles/
│       └── global.css            ← Tailwind base + custom CSS vars
├── astro.config.mjs
├── tailwind.config.mjs
├── package.json
└── tsconfig.json
```

---

## Bootstrap Commands

```bash
# 1. Scaffold the project
npm create astro@latest portfolio-website -- --template minimal --typescript strict

cd portfolio-website

# 2. Add Tailwind
npx astro add tailwind

# 3. Install Inter + JetBrains Mono (via @fontsource for self-hosting)
npm install @fontsource/inter @fontsource/jetbrains-mono

# 4. Dev server
npm run dev   # → http://localhost:4321
```

---

## Vercel Deployment

1. Push the repo to GitHub (new repo, e.g. `youruser/portfolio`)
2. Go to vercel.com → New Project → Import from GitHub
3. Vercel auto-detects Astro — no config needed
4. Set custom domain later (optional)
5. Every `git push` to `main` triggers a redeploy automatically

---

## Content To-Do Before First Deploy

- [ ] Upload `Jonathan_Santoso_Resume.pdf` → `public/cv.pdf`
- [ ] Confirm public GitHub username → update all 8 project repo links
- [ ] Add LinkedIn URL
- [ ] Add real Springer DOI link for publication
- [ ] Add favicon (initials "JS" or a simple data icon as SVG)

---

## What NOT To Build

- No blog / CMS (keep it simple)
- No contact form with backend (just an email link)
- No dark/light mode toggle (dark only — simpler, looks better)
- No JavaScript frameworks (Astro ships zero JS — keep it that way)
- No animations library (vanilla CSS transitions only)
