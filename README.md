# Ledger — Portfolio & Research Dashboard

A consolidated equity research and portfolio tracking dashboard built for self-directed retail traders. Combines portfolio P/L tracking, filtered news, an earnings/catalyst calendar, sector allocation, and SEC Form 4 insider activity into a single focused workflow.

> **Live demo:** [your-vercel-url-here.vercel.app](#)
> **Status:** Prototype / v1

---

## Why I built this

I trade actively and was tired of jumping between Robinhood (positions), Yahoo Finance (news/charts), Finviz (screening), and the SEC EDGAR website (filings/insider trades). Each tool does one thing — none of them stitch the workflow together. The goal of Ledger is to be the single tab a retail trader keeps open.

## Screenshots

![Dashboard](./screenshots/dashboard.png)

## Features

- **Portfolio dashboard** — total value, day P/L, all-time P/L, performance chart with benchmark comparison
- **Holdings table** — positions with avg cost, market value, realized/unrealized P/L, and portfolio weight
- **Filtered news feed** — news prioritized by your holdings and watchlist, with sentiment indicators
- **Catalyst calendar** — upcoming earnings, FDA AdComs, Fed meetings, and other event-driven catalysts
- **Sector allocation** — concentration breakdown to monitor risk
- **SEC Form 4 insider activity** — recent buys/sells by officers and 10% holders for your tickers
- **Watchlist** — track tickers you're researching with inline sparklines
- **Command palette** — ⌘K to search tickers, news, and filings (UI complete)

## Tech Stack

**v1 (current):** Vanilla HTML/CSS/JS + Chart.js — single-file static prototype, deployable anywhere in 30 seconds.

**v2 (planned):** Next.js 14 + TypeScript + Tailwind + shadcn/ui, with:
- Finnhub free tier for real-time quotes, news, and earnings dates
- SEC EDGAR (no API key required) for Form 4 insider transactions and 13F filings
- Supabase for portfolio persistence
- Redis (Upstash) for response caching to stay within data provider rate limits
- Vercel deployment

## Architecture (planned for v2)

```
┌────────────────────────────────────────────────────────────┐
│                    Next.js App Router                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐ │
│  │  Dashboard   │  │   Research   │  │  Catalyst Cal.   │ │
│  └──────────────┘  └──────────────┘  └──────────────────┘ │
└────────────────────────────────────────────────────────────┘
                            │
                ┌───────────┴───────────┐
                ▼                       ▼
        ┌───────────────┐       ┌───────────────┐
        │  Server       │       │  Server       │
        │  Components   │       │  Actions      │
        └───────┬───────┘       └───────┬───────┘
                │                       │
                ▼                       ▼
        ┌────────────────────────────────────────┐
        │       Redis Cache (Upstash)            │
        │   Quotes 60s · Fundamentals 24h        │
        └────────────────────────────────────────┘
                │
        ┌───────┼───────┐
        ▼       ▼       ▼
    ┌──────┐ ┌─────┐ ┌────────┐
    │ Finn │ │EDGAR│ │ Alpha  │
    │ hub  │ │     │ │Vantage │
    └──────┘ └─────┘ └────────┘
                │
                ▼
        ┌───────────────┐
        │   Supabase    │
        │   Postgres    │
        └───────────────┘
```

## Design Decisions

- **Dark mode default.** Every trader uses dark mode. Light mode is a v2 add.
- **Tabular numerics.** All financial figures use `font-feature-settings: 'tnum'` so columns of numbers align character-by-character.
- **Typography.** Fraunces for display (warm, editorial — counterbalances the data density), JetBrains Mono for all numbers and tickers, Inter for UI text.
- **Color semantics.** Green/red exclusively reserved for gains/losses — never decorative. The lime-green accent is used for primary calls-to-action and the active brand mark.
- **Information density.** Borrowed from Linear and Bloomberg Terminal — generous space *between* sections, but tight data *within* tables and cards.

## Roadmap

- [ ] Migrate to Next.js + TypeScript
- [ ] Wire up Finnhub for live quotes (60s polling)
- [ ] SEC EDGAR integration for real Form 4 + 13F data
- [ ] User auth + persistent portfolios via Supabase
- [ ] Per-ticker research page with TradingView lightweight charts
- [ ] CSV import (Robinhood / Schwab export format)
- [ ] Trade journal with thesis tracking
- [ ] LLM-based news sentiment classifier (replace keyword heuristic)

## Running locally

It's one HTML file. Open `index.html` in a browser. That's it.

For deployment: drag the folder onto Netlify, push to a repo and connect Vercel, or `npx serve .`.

## Out of scope (v1)

- No real brokerage integration — manual entry / CSV import only
- No options or futures
- No multi-user / multi-currency
- No real-time streaming (REST polling is sufficient at this scale)

---

Built by Ryan, May 2026.
