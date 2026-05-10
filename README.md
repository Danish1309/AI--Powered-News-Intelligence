# 🧠 NewsX — AI-Powered News Intelligence Platform

> A futuristic, cinematic news intelligence dashboard that ingests global headlines, enriches them with **Groq LLM** sentiment & insights, and presents them through an immersive **3D + glassmorphism** UI built with **TanStack Start**, **React Three Fiber**, and **Supabase**.

<p align="center">
  <img src="https://img.shields.io/badge/React-19-61dafb?logo=react&logoColor=white" />
  <img src="https://img.shields.io/badge/TanStack_Start-1.x-ff4154?logo=react-query&logoColor=white" />
  <img src="https://img.shields.io/badge/TypeScript-5-3178c6?logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailwind-4-38bdf8?logo=tailwindcss&logoColor=white" />
  <img src="https://img.shields.io/badge/Supabase-Edge-3ecf8e?logo=supabase&logoColor=white" />
  <img src="https://img.shields.io/badge/Groq-LLM-f55036" />
  <img src="https://img.shields.io/badge/Three.js-R3F-000000?logo=three.js&logoColor=white" />
</p>

<p align="center">
  <a href="https://ai-powered-news-intelligence.lovable.app"><strong>🌐 Live Preview → ai-powered-news-intelligence.lovable.app</strong></a>
</p>

---

## ✨ Project Overview

**NewsX** is a production-grade, recruiter-portfolio-quality web application that transforms raw global news into actionable intelligence. It pulls fresh articles from **NewsData.io**, runs each through a **Groq-hosted LLM** (`llama-3.1-8b-instant`) to generate summaries, sentiment classifications, confidence scores, and structured insights — then renders the results in a luxurious, animated dashboard with **3D orbs**, **network globes**, **particle fields**, and **mouse-tilt cards**.

The result is a news experience that feels less like an RSS reader and more like a *Bloomberg terminal designed by Apple*.

---

## 🚀 Features

### Intelligence
- 🤖 **AI Summaries** — Each article condensed to 1–2 razor-sharp sentences
- 💬 **Sentiment Analysis** — Positive / Neutral / Negative with color-coded UI
- 📊 **Confidence Scores** — Per-article model confidence (0–100%)
- 🔍 **Structured Insights** — 3–5 bullet-point takeaways per article
- 📈 **Live Analytics Panel** — Aggregated sentiment distribution, source diversity, processing rates

### Experience
- 🌌 **3D Hero Orb** — Distorted icosahedron with emissive shader (R3F + drei)
- 🌐 **Network Globe** — Animated 3D node graph
- ✨ **Particle Field** — Canvas-based ambient violet/cyan particles
- 🎴 **Tilt Cards** — Mouse-tracking 3D perspective on every card with radial glow
- 🪟 **Glassmorphism** — Frosted, layered surfaces with aurora gradients
- 🎬 **Cinematic Page Transitions** — Framer Motion blur+slide entrances
- 🌊 **Lenis Smooth Scroll** — Buttery 1.15s easing across the entire app
- 📱 **Fully Responsive** — Mobile → 4K
- 🔖 **Bookmarks** — Session-based article saving
- 🎚️ **Filters** — Category, source, sentiment, search, sort

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Framework** | TanStack Start v1 (SSR + Server Functions) |
| **Build** | Vite 7 |
| **UI** | React 19 + TypeScript 5 |
| **Styling** | Tailwind CSS v4 + shadcn/ui |
| **3D** | React Three Fiber + drei + Three.js |
| **Motion** | Framer Motion + GSAP + Lenis |
| **Backend** | Supabase (Postgres + Edge Functions + RLS) |
| **AI** | Groq API (`llama-3.1-8b-instant`) |
| **News** | NewsData.io |
| **Deploy** | Cloudflare Workers (Edge) |

---

## 🏗️ Architecture Overview

```text
┌─────────────────────────────────────────────────────────────┐
│                        BROWSER                              │
│  React 19 · R3F · Framer Motion · Lenis · TanStack Router   │
└────────────────────┬────────────────────────────────────────┘
                     │ HTTPS / Server Functions
┌────────────────────▼────────────────────────────────────────┐
│          TANSTACK START (Cloudflare Worker / Edge)          │
│         SSR · Routing · createServerFn · API Routes         │
└────────────────────┬────────────────────────────────────────┘
                     │
       ┌─────────────┼──────────────────┐
       ▼             ▼                  ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│   SUPABASE   │ │  GROQ LLM    │ │  NEWSDATA    │
│  Postgres +  │ │ llama-3.1-8b │ │   .io API    │
│ Edge Funcs + │ │   instant    │ │              │
│     RLS      │ └──────────────┘ └──────────────┘
└──────────────┘
```

---

## 🤖 AI Workflow

```text
1. CRON / manual trigger  →  supabase/functions/ingest-news
2. Fetch latest from NewsData.io (filter by category/lang)
3. Deduplicate against `articles.external_id`
4. For each fresh article:
     ├─ Build prompt: title + description + content (1500 chars)
     ├─ POST → Groq (response_format: json_object, temp 0.3)
     └─ Parse JSON → { summary, sentiment, insights[], confidence }
5. UPSERT into `articles` table (RLS-protected)
6. Frontend polls / re-fetches → renders in animated grid
```

The Groq prompt enforces strict JSON output via `response_format`, keeping ingestion deterministic and parse-safe.

---

## 🗄️ Supabase Setup

1. Create a project at [supabase.com](https://supabase.com)
2. Run the migration in `supabase/migrations/` to create:
   - `articles` table (with `external_id` unique constraint)
   - `bookmarks` table (session-scoped)
   - RLS policies for public read + session-scoped writes
3. Deploy the edge function:
   ```bash
   supabase functions deploy ingest-news
   ```
4. Set secrets in **Project Settings → Edge Functions**:
   - `NEWSDATA_API_KEY`
   - `GROQ_API_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY` (auto-provided)

---

## 🔑 Groq API Setup

1. Sign up at [console.groq.com](https://console.groq.com)
2. Create an API key under **API Keys**
3. Add it to Supabase Edge Function secrets as `GROQ_API_KEY`
4. Model used: **`llama-3.1-8b-instant`** (fast, cheap, JSON-mode capable)

---

## 📰 NewsData API Setup

1. Register at [newsdata.io](https://newsdata.io)
2. Grab your API key from the dashboard
3. Add it to Supabase Edge Function secrets as `NEWSDATA_API_KEY`
4. Free tier supports 200 credits/day — sufficient for demos

---

## 📦 Installation

```bash
# Clone
git clone https://github.com/your-username/newsx.git
cd newsx

# Install deps (Bun recommended)
bun install
# or
npm install
```

---

## 🔐 Environment Variables

Create `.env` at the project root:

```env
VITE_SUPABASE_URL="https://<project>.supabase.co"
VITE_SUPABASE_PUBLISHABLE_KEY="<anon-key>"
VITE_SUPABASE_PROJECT_ID="<project-id>"

# Server-only (do NOT prefix with VITE_)
SUPABASE_URL="https://<project>.supabase.co"
SUPABASE_PUBLISHABLE_KEY="<anon-key>"
SUPABASE_SERVICE_ROLE_KEY="<service-role-key>"
```

Edge function secrets (set in Supabase dashboard, **not** in `.env`):
- `GROQ_API_KEY`
- `NEWSDATA_API_KEY`

---

## 💻 Local Development

```bash
bun run dev        # Start Vite dev server (http://localhost:8080)
bun run build      # Production build
bun run preview    # Preview production build
```

To trigger ingestion locally:
```bash
curl -X POST https://<project>.supabase.co/functions/v1/ingest-news \
  -H "Content-Type: application/json" \
  -d '{"category":"technology"}'
```

---

## ☁️ Deployment

The app deploys to **Cloudflare Workers** via the TanStack Start adapter:

```bash
bun run build
wrangler deploy
```

Or one-click deploy via **Lovable**:
```text
https://ai-powered-news-intelligence.lovable.app
```

Schedule the ingest function via **pg_cron** or any external scheduler hitting:
```
POST /functions/v1/ingest-news
```

## 🔮 Future Improvements

- [ ] 🔐 Full Supabase Auth (Google / GitHub OAuth)
- [ ] 🗣️ Multi-language ingestion + translation layer
- [ ] 🧵 Topic clustering with embeddings (pgvector)
- [ ] 📰 Personalized feed via user preference learning
- [ ] 📨 Daily AI digest email
- [ ] 📊 Historical sentiment trends (time-series charts)
- [ ] 🎙️ Text-to-speech briefings (ElevenLabs)
- [ ] 🔔 Real-time push notifications (Supabase Realtime)
- [ ] 🤝 Shareable bookmark collections

---

## 🛣️ API Routes & Server Functions

| Endpoint | Type | Purpose |
|---|---|---|
| `/` | Page | Landing with 3D hero |
| `/dashboard` | Page | Filterable article grid |
| `supabase/functions/ingest-news` | Edge Function | Fetch + enrich + persist |
| `articles` (Supabase REST) | DB | Public read |
| `bookmarks` (Supabase REST) | DB | Session-scoped CRUD |

---

## 📁 Folder Structure

```text
src/
├── components/
│   ├── dashboard/         # ArticleCard, Drawer, FilterBar, Analytics
│   ├── effects/           # TiltCard, ParticleField, SmoothScroll, PageTransition
│   ├── three/             # HeroOrb, NetworkGlobe (R3F)
│   ├── ui/                # shadcn primitives
│   ├── ClientOnly.tsx
│   └── NavBar.tsx
├── integrations/
│   └── supabase/          # client, client.server, types, auth-middleware
├── lib/
│   ├── articles.ts        # Article fetchers
│   ├── bookmarks.ts       # Bookmark CRUD
│   └── sessionId.ts       # Anonymous session id
├── routes/
│   ├── __root.tsx         # Shell + providers
│   ├── index.tsx          # Landing
│   └── dashboard.tsx
├── styles.css             # Design tokens (oklch) + keyframes
└── router.tsx

supabase/
├── functions/ingest-news/ # Groq enrichment edge function
├── migrations/            # SQL schema
└── config.toml
```

---

## 🎨 Animations & UI Highlights

| Effect | Library | Where |
|---|---|---|
| Mouse-tilt 3D cards | Framer Motion springs | Article + analytics + feature cards |
| Radial mouse-glow | Framer `useTransform` | TiltCard hover |
| Distorted hero orb | R3F + `MeshDistortMaterial` | Landing |
| Animated network globe | R3F | Landing |
| Ambient particles | Canvas 2D + RAF | App-wide background |
| Lenis smooth scroll | Lenis | Global (drawer-isolated) |
| Aurora drifting blob | CSS `@keyframes` | Backgrounds |
| Cinematic page entry | Framer Motion blur+slide | Per-route |
| Glassmorphism | `backdrop-blur` + oklch tokens | Cards, drawer, nav |
| Glowing borders | Box-shadow on hover | TiltCard |

---

## 📜 License

MIT — built with ❤️ for engineers, and news junkies. BY Danish Shaikh.

---

<p align="center">
  <strong>If this project impressed you, drop a ⭐ on the repo.</strong>
</p>
