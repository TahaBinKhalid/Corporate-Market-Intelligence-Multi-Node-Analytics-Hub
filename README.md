# 💠 NexusAI: 3D Corporate Market Intelligence Hub

An enterprise-grade, glassmorphic financial telemetry and market intelligence web application built to deliver real-time corporate data visualization and multi-node metric analytics.

---

## 📖 Overview

**NexusAI** is a next-generation financial intelligence dashboard that transforms raw market data into a cinematic, interactive experience. Designed for analysts, executives, and data-driven decision-makers, it combines **live market feeds** from major public companies with **volumetric 3D visualizations** and a **premium glassmorphic UI** — all running on a lightweight Streamlit backend.

Unlike traditional dashboards that rely on static spreadsheets or stale snapshots, NexusAI pulls **real-time corporate telemetry** via `yFinance`, computes live KPIs (market cap, TTM revenue, profit margins, P/E ratios), and renders them inside a fully interactive 3D WebGL canvas. Every filter, every toggle, and every export reflects the **current** state of the market.

The application is optimized for **zero-configuration deployment** — clone the repo, install dependencies, and launch. It runs locally, in Google Colab, or on any cloud VM with a single command.

---

## 🚀 Key Features

### 1. Real-World Corporate Telemetry
- **Direct integration** with live market data feeds via `yFinance` (no API key required).
- Tracks **top-tier enterprises** out of the box: **Apple (AAPL), Microsoft (MSFT), Google (GOOGL), NVIDIA (NVDA), Tesla (TSLA), Amazon (AMZN)**.
- Auto-refreshing data pipeline with **5-minute cache TTL** to balance freshness and rate-limit safety.
- Gracefully degrades to the most recent cached snapshot if the upstream feed is temporarily unavailable.

### 2. 3D Glassmorphic UI
- **Modern dark-mode aesthetic** with a deep navy-to-slate gradient background and animated ambient glows.
- **Translucent glass cards** using `backdrop-filter: blur()` for true frosted-glass depth.
- **Neon accent badges** (cyan, violet, emerald) that highlight live status, category, and trend direction.
- **Smooth interactive hover effects** — cards lift, borders glow, and shimmer sweeps animate on hover.
- Fully **responsive grid layout** that collapses from 4-column KPIs to 2-column on smaller viewports.

### 3. Advanced Visualizations
- **Dynamic Plotly 2D area waveform charts** — smooth cumulative value curves with gradient fills, ideal for tracking intraday or multi-day price velocity.
- **Volumetric 3D scatter projections** — Plotly WebGL-powered 3D plot mapping **Market Cap × Revenue × Profit Margin** in a single spatial view. Rotate, zoom, and hover to inspect any enterprise.
- **Category bar charts** — side-by-side comparison of company distribution across sectors.
- **Interactive legends** — click any trace to isolate a single company across all charts simultaneously.

### 4. Executive KPI Metric Rows
- **Real-time tracking** of four flagship metrics:
  - **Current Valuation** — live market cap in USD.
  - **Market Capitalization** — snapshot with delta from previous close.
  - **Annual Revenue (TTM)** — trailing-twelve-month total revenue.
  - **Net Profit Margin** — net income as a percentage of revenue.
- Each KPI card includes a **trend indicator** (up/down arrow with percentage delta).
- Staggered `fadeUp` animations on page load for a cinematic reveal.

### 5. Encrypted Report Export
- **Instant one-click CSV download** of the fully filtered dataset.
- Export includes all computed fields: name, ticker, market cap, revenue, net margin, P/E ratio, sector, and last refresh timestamp.
- **SHA-256 integrity hash** printed alongside the download button, so recipients can verify the export hasn't been tampered with.
- Filename is timestamped (`nexusai_export_YYYYMMDD_HHMMSS.csv`) for audit trail continuity.

---

## 🛠️ Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Core Language** | Python 3.10+ | Application runtime |
| **Web Interface Framework** | [Streamlit](https://streamlit.io/) ≥ 1.32 | Reactive UI, layout, session state |
| **Data Visualization** | [Plotly](https://plotly.com/python/) ≥ 5.18 | 2D area charts + 3D WebGL scatter |
| **Data Processing & APIs** | [Pandas](https://pandas.pydata.org/) ≥ 2.0, [yFinance](https://github.com/ranaroussi/yfinance) ≥ 0.2.40 | Live market data ingestion, ETL, metrics |
| **Styling** | Custom CSS injected via `st.markdown(unsafe_allow_html=True)` | Glassmorphism, animations, theming |
| **Deployment** | Local / Google Colab / Any VM | Cloudflared tunnel for public URL exposure |

### Why these choices?

- **`yFinance` over paid APIs** — free, no key required, covers all major public companies, and returns TTM fundamentals out of the box.
- **Plotly over Matplotlib** — native 3D WebGL rendering, interactive hover, and far better visual quality for a "premium" aesthetic.
- **Streamlit over Flask/Dash** — reactive rendering without writing JavaScript, single-file app, ideal for rapid iteration and Colab compatibility.

---

## 📂 Project Structure

```text
nexus-ai-market-hub/
│
├── app.py                  # Main Streamlit 3D glassmorphic application
├── requirements.txt        # Project dependencies
├── assets/                 # UI icons and styling sheets
│   ├── icons/
│   │   ├── nexus_logo.svg
│   │   ├── kpi_revenue.svg
│   │   ├── kpi_marketcap.svg
│   │   ├── kpi_margin.svg
│   │   └── kpi_valuation.svg
│   └── styles/
│       ├── glass.css       # Glassmorphic card styles
│       ├── animations.css  # FadeUp, pulse, shimmer keyframes
│       └── palette.css     # Design token definitions
│
├── data/
│   ├── cache/              # yFinance response cache (auto-managed)
│   └── samples/
│       └── sample_export.csv
│
├── tests/
│   ├── test_metrics.py     # Unit tests for KPI computation
│   └── test_export.py      # CSV integrity + hash validation
│
├── .streamlit/
│   └── config.toml         # Dark theme + primary color overrides
│
├── .gitignore              # Excludes cache/, *.pyc, secrets
├── LICENSE                 # MIT
└── README.md               # Project documentation (this file)
```

---

## ⚡ Quick Start

### 1. Clone & Install

```bash
git clone https://github.com/your-org/nexus-ai-market-hub.git
cd nexus-ai-market-hub
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Configure Theme (optional)

Create `.streamlit/config.toml`:

```toml
[theme]
base = "dark"
primaryColor = "#3B82F6"
backgroundColor = "#0B1120"
secondaryBackgroundColor = "#111827"
textColor = "#F1F5F9"
```

### 3. Run

```bash
streamlit run app.py
```

Open `http://localhost:8501`.

### 4. Run in Google Colab (no local setup)

```python
!pip install -q streamlit yfinance plotly pandas

# Paste the contents of app.py into a file, then:
!nohup streamlit run app.py --server.port 8501 --server.headless true > /tmp/st.log 2>&1 &

# Expose via cloudflared
!wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64 -O /tmp/cloudflared
!chmod +x /tmp/cloudflared
! /tmp/cloudflared tunnel --url http://localhost:8501
```

Copy the `https://xxx.trycloudflare.com` URL printed in the output.

---

## 📊 Usage Guide

### Filtering the Universe
The **left sidebar** provides live filters:
- **Company selector** — multi-select from the default six, or add any ticker (e.g. `META`, `AMD`, `JPM`).
- **Sector filter** — Technology, Automotive, Consumer Discretionary, Financials.
- **Time window** — 1D, 5D, 1M, 3M, 6M, 1Y, YTD.

### Reading the KPIs
The top **KPI row** displays four real-time metrics for the currently selected universe:
- **Current Valuation** — sum of market caps across selected companies.
- **Market Cap (Avg)** — mean market capitalization.
- **Revenue (TTM)** — total trailing-twelve-month revenue.
- **Net Margin (Avg)** — mean net profit margin across the universe.

### Exploring the 3D Plot
- **Drag** to rotate the 3D scene.
- **Scroll** to zoom.
- **Hover** any bubble to see the company name, market cap, revenue, and margin.
- **Click legend items** to isolate a single trace.

### Exporting
- Click **📥 Export Enterprise Report (CSV)** to download the current filtered view.
- The **SHA-256 hash** below the button lets you verify integrity post-download.

---

## 🧪 Testing

```bash
pytest tests/
```

Coverage targets:
- `test_metrics.py` — KPI computation matches manual calculation for a 3-company fixture.
- `test_export.py` — CSV bytes hash matches the value displayed in the UI.

---

## 🎨 Design System

| Token | Value | Usage |
|---|---|---|
| `--bg-0` | `#0B1120` | Base background |
| `--bg-1` | `#111827` | Panel background |
| `--accent` | `#3B82F6` | Primary blue (KPIs, CTAs) |
| `--accent-2` | `#8B5CF6` | Secondary violet (gradients) |
| `--success` | `#10B981` | Positive deltas |
| `--warn` | `#F59E0B` | Neutral / attention |
| `--danger` | `#EF4444` | Negative deltas |
| `--radius` | `14px` | Card corner radius |
| `--blur` | `18px` | Glass backdrop-filter |

Animations: `fadeUp` (0.6s), `pulse` (2s), `shimmer` (6s), `spin` (14s).

---

## 🚢 Deployment Options

| Environment | Command | Notes |
|---|---|---|
| **Local** | `streamlit run app.py` | Fastest; `localhost:8501` |
| **Google Colab** | Cloudflared tunnel | Public HTTPS URL, no signup |
| **Render** | `render.yaml` | Free tier supports Streamlit |
| **Railway** | `railway up` | Auto-detects Streamlit |
| **Docker** | `docker build -t nexusai . && docker run -p 8501:8501 nexusai` | Fully portable |

A sample `Dockerfile`:

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8501
CMD ["streamlit", "run", "app.py", "--server.port=8501", "--server.address=0.0.0.0"]
```

---

## ⚠️ Known Limitations

- **yFinance rate limits** — Yahoo Finance throttles aggressive polling. The built-in 5-minute cache mitigates this, but spamming "Refresh" can trigger temporary bans.
- **Fundamentals lag** — TTM revenue and net income update quarterly, not intraday. Market cap and price update in near real-time.
- **3D rendering** — Requires WebGL-capable browser. Older Safari versions may render the 3D scatter at reduced quality.
- **No persistent user state** — This is a stateless dashboard. For saved watchlists or multi-user auth, integrate with a database backend (Postgres + Supabase recommended).

---

## 🗺️ Roadmap

- [ ] **Watchlist persistence** via Supabase / SQLite
- [ ] **Earnings calendar overlay** on the 2D area chart
- [ ] **Sentiment analysis** from recent news headlines (yFinance `.news` + TextBlob)
- [ ] **Portfolio simulation** — "what if I invested $10k in these 3 companies 5 years ago?"
- [ ] **Dark/light theme toggle** with persisted preference
- [ ] **PDF report generation** via `reportlab` (replacing CSV-only export)
- [ ] **Webhook alerts** — Slack/email notifications when a KPI crosses a threshold

---

## 🤝 Contributing

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/my-feature`.
3. Commit changes: `git commit -m "feat: add new KPI"`.
4. Push: `git push origin feature/my-feature`.
5. Open a Pull Request with a description of the change and a screenshot if UI-related.

Please follow [Conventional Commits](https://www.conventionalcommits.org/) and run `pytest` before submitting.

---

## 📄 License

MIT © 2026 NexusAI Contributors. See [LICENSE](LICENSE) for details.

---

## 🙏 Acknowledgments

- Market data: [Yahoo Finance](https://finance.yahoo.com/) via [yFinance](https://github.com/ranaroussi/yfinance)
- Charts: [Plotly](https://plotly.com/python/)
- Framework: [Streamlit](https://streamlit.io/)
- Design inspiration: Glassmorphic design systems (Apple visionOS, Linear, Vercel)

---

⚙️ Quick Start & Installation
1. Clone the Repository
Bash
git clone [https://github.com/TahaBinKhalid/nexus-ai-market-hub.git](https://github.com/TahaBinKhalid/nexus-ai-market-hub.git)
cd nexus-ai-market-hub
2. Set Up a Virtual Environment
Bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
3. Install Dependencies
Bash
pip install -r requirements.txt
4. Run the Application
Launch the Streamlit interface locally:

Bash
streamlit run app.py
Open your browser and navigate to http://localhost:8501.

💡 Use Cases
Executive Financial Analysis: Monitoring real-time stock movements and quarterly valuation metrics.

Client Portfolios: Showcasing high-end UI/UX engineering combined with complex Python data pipelines on Upwork and Fiverr.

Algorithmic Trading & Research: Evaluating asset volatility and trend patterns using volumetric 3D projections.

👤 Author
Developed by Muhammad Taha Bin Khalid

AI Software Engineer & Full-Stack Developer

---

> **NexusAI** — *Where market data becomes market intelligence.*
