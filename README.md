# 🇮🇩 IDX Top Investor Dashboard

**An interactive web dashboard mapping significant shareholders (>1% ownership) across all IDX-listed companies, built from official KSEI (Kustodian Sentral Efek Indonesia) data.**

🔗 **[Live Dashboard →](https://your-username.github.io/idx-investor-dashboard/)**

---

## 📌 Overview

This dashboard provides a comprehensive view of institutional and individual investors who hold more than 1% of shares in any company listed on the Indonesia Stock Exchange (IDX/BEI). It enables analysts, researchers, and investors to:

- Identify who the major shareholders are across the entire IDX universe
- Understand how investors diversify across sectors and companies
- Map the ownership network between investors and stocks
- Track changes in significant shareholding over time (Jan → Feb → Mar 2026)
- Visualise conglomerate ownership across Indonesian business groups

### Key Statistics (as of 31 March 2026)

| Metric | Value |
|---|---|
| IDX-listed stocks covered | **955 companies** |
| Unique investors with >1% holdings | **5,176 investors** |
| Total investor–stock positions tracked | **7,213 holdings** |
| Conglomerate groups mapped | **33 groups** |
| Data source | KSEI Balance Position Report + Named Shareholder PDF |
| Data date | 31 March 2026 (trend data from Jan & Feb 2026) |

---

## 🗂️ Data Sources

All data is sourced directly from **KSEI (Kustodian Sentral Efek Indonesia)**, the official central securities depository of Indonesia.

| File | Description |
|---|---|
| `Balancepos_Kepemilikan Efek.txt` | Daily balance position file — shareholding breakdown per stock by investor category (Local/Foreign × Institution type). Used for Jan & Feb aggregate ownership. |
| `StatisEfek_Master File Efek.txt` | Master security file — stock metadata including company name, sector, listing date, and closing price |
| `Pemegang Saham di atas 1% (KSEI).pdf` | KSEI's official report listing all named shareholders holding >1% of any IDX-listed company. Available for Feb and Mar 2026. |

> Data files follow the pipe-delimited (`|`) format as published by KSEI's C-BEST system. The PDF reports are processed using `pdfplumber` to extract named investor records.

---

## 🖥️ Dashboard Features

The dashboard is a **single self-contained HTML file** (~2.4 MB) with embedded data — no backend or build step required. It uses [D3.js v7](https://d3js.org/) for all visualisations and [SheetJS](https://sheetjs.com/) for Excel export.

### Display Modes

A floating **☀️ / 🌙 toggle button** (bottom-right corner) switches between **Night Mode** (default, dark background) and **Day Mode** (light background). All charts, tables, and network graphs adapt automatically.

### Tab 1 — 🌐 Overview

A high-level summary with four interactive charts:

- **Top 20 Investors by Number of Holdings** — horizontal bar chart ranking investors by the number of IDX companies they hold >1% in; hover to see total value and sector diversification
- **Top 15 Sectors by Investor Count** — shows which industries attract the most significant shareholders
- **Local vs Foreign Distribution** — donut chart of domestic versus foreign investor positions; hover slices to see exact counts
- **Investor Type Distribution** — breakdown by entity type (Corporation, Individual, Mutual Fund, Pension Fund, Insurance, Securities Company, Foundation, Others); hover slices to expand details

### Tab 2 — 👥 Top Investors

A sortable, filterable table of up to 500 significant investors:

- **Columns:** Investor Name · Stocks Held · Unique Sectors · Total Holding Value (IDR) · Investor Type · Local/Foreign
- **Filters:** Search by name, filter by investor type or Local/Foreign
- **Click any row** to expand the investor's full portfolio below the table — showing every stock they hold >1% in, with ownership percentage, holding value, and sector
- **Export** the full investor list to Excel (.xlsx) or CSV with one click

### Tab 3 — 📋 All Holdings

A complete searchable table of all 7,213 individual investor–stock positions:

- Search simultaneously across stock code, company name, or investor name
- Filter by sector or Local/Foreign
- Sort by any column (%, shares, value, etc.)
- Paginated 50 rows at a time
- **Export** filtered results to Excel or CSV

### Tab 4 — 🕸️ Network Map

An interactive force-directed network graph visualising ownership relationships:

- **Search** by stock code (e.g. `BUMI`, `BBCA`) or investor name — live dropdown with matches
- **Default view:** BUMI and its investor network
- **3 selectable relationship layers:**
  - **Layer 1** — Selected stock → its direct investors
  - **Layer 2** — Layer 1 investors' other stock holdings (cross-holdings)
  - **Layer 3** — Other investors in those Layer 2 stocks
- **Node shapes:** Diamond = Stock · Circle = Investor
- **Node colours by layer:** Gold (root stock) · Blue (L1 investors) · Orange (L2 stocks) · Green/Purple (L3)
- **Edge width** scales with ownership percentage; edge labels show % on primary connections
- **Node labels** display stock codes and investor names directly on the graph
- **Click any node** to highlight its connections and dim the rest; click the canvas to reset

### Tab 5 — 📊 Sector Analysis

Sector-level breakdown with interactive charts:

- Horizontal bar chart of all sectors ranked by number of stocks with significant (>1%) shareholders
- **Hover** any bar to see investor count and total holding value
- **Click any bar or table row** to instantly generate a detailed stock list for that sector — showing all stocks, how many investors hold >1%, key investor names, and total holding value
- **Export** the sector stock list to Excel or CSV

### Tab 6 — 📈 Trend Tracker

Tracks changes in significant shareholding across two comparable periods:

- **Period selector:** Feb → Mar 2026 (default) or Jan → Feb 2026
- **New Entries** — investors who newly crossed the 1% threshold between periods
- **Exits** — investors who dropped below 1% or exited entirely
- **Notable Changes** — investors whose stake shifted by ≥0.5 percentage points
- **January aggregate column** — for the Feb → Mar view, shows the January local/foreign aggregate ownership % sourced from the Balancepos file (note: this is category-level, not individual named investor level)
- **Export** each section to Excel or CSV

### Tab 7 — 🏢 Conglomerate Map

Maps IDX-listed companies to their Indonesian conglomerate owners:

- **33 conglomerate groups** identified from actual KSEI-reported holding company names, including Hartono (Djarum/BCA), Widjaja (Sinar Mas), Wonowidjojo (Gudang Garam), Bakrie, Salim, Riady (Lippo), and others
- **Bubble chart** showing each group's relative IDX holding value
- **Click any bubble** to see a full breakdown of all positions within that group — stock, ownership %, holding value, and sector
- **Conglomerate cards** summarise each group's total value, number of stocks held, and affiliated entities

> **Note on methodology:** Conglomerate assignment is based on the legal holding company names as reported by KSEI. Some groups use multiple holding entities (e.g. the Hartono family holds BCA through *PT Dwimuria Investama Andalan*, distinct from other Djarum group entities). The Wonowidjojo family (Gudang Garam) is mapped separately from the Hartono/Djarum group. Positions held via nominee or custodian accounts may not be captured.

---

## 🚀 Getting Started

### Option A — View Locally

Download `idx_investor_dashboard.html` and open it directly in any modern browser (Chrome, Firefox, Edge, Safari). No installation needed. An internet connection is required only to load D3.js from CDN.

### Option B — Deploy to GitHub Pages

1. Fork or clone this repository
2. Rename `idx_investor_dashboard.html` → `index.html`
3. Go to **Settings → Pages → Source** and select `main` branch, root `/`
4. Your dashboard will be live at `https://your-username.github.io/idx-investor-dashboard/`

> All data is embedded in the HTML file (~2.4 MB). No server-side code or database is required.

---

## 🔄 Updating the Data

To refresh with new KSEI data:

1. Download the latest files from the KSEI website:
   - Balance Position (`Balancepos_*.txt`)
   - Master Security File (`StatisEfek_*.txt`)
   - Shareholder PDF (`Pemegang Saham di atas 1%_*.pdf`) — for the two most recent months

2. Run the extraction scripts:
```bash
pip install pdfplumber pandas openpyxl
python extract_pdf.py       # Extracts named investor data from the PDF
python prepare_data.py      # Merges with master file, builds investor JSON
python extract_trends2.py   # Computes period-over-period changes (new/exit/change)
python build_cong_v2.py     # Builds conglomerate mapping data
python build_dashboard.py   # Generates the full dashboard HTML
```

3. The output `idx_investor_dashboard.html` is ready to view or deploy.

---

## 📐 Technical Notes

| Item | Detail |
|---|---|
| Frontend | Vanilla HTML + CSS + JavaScript |
| Visualisation library | D3.js v7.8.5 (CDN) |
| Excel export | SheetJS xlsx.js v0.18.5 (CDN) |
| Data format | JSON embedded in HTML (~2.4 MB total file) |
| Network layout | D3 Force Simulation with collision, charge, and link forces |
| PDF extraction | `pdfplumber` Python library |
| Theme | CSS custom properties with `body.light` class toggle |
| Browser support | Chrome 90+, Firefox 88+, Edge 90+, Safari 14+ |

### Investor Type Codes (KSEI)

| Code | Description |
|---|---|
| CP | Corporation |
| ID | Individual |
| MF | Mutual Fund |
| IS | Insurance |
| IB | Insurance / Pension Fund (combined) |
| PF | Pension Fund |
| SC | Securities Company |
| FD | Foundation (Yayasan) |
| OT | Others |

### Conglomerate Groups (33 total)

Selected groups included in the mapping:

| Group | Key Holding Entity | Major Stakes |
|---|---|---|
| Hartono / Djarum | PT Dwimuria Investama Andalan | BBCA |
| Wonowidjojo / Gudang Garam | PT Suryaduta Investama | GGRM |
| Widjaja / Sinar Mas | PT Sinarmas Sekuritas, APP entities | SMGR, INKP, TKIM |
| Bakrie | PT Bakrie & Brothers | BUMI, UNSP, BRMS |
| Salim | PT Indofood, PT Indomobil | ICBP, INDF, IMAS |
| Riady / Lippo | PT Lippo Karawaci | LPKR, SILOAM |
| Prajogo Pangestu | PT Barito Pacific | BREN, TPIA, BRPT |
| … and 26 more | | |

---

## ⚠️ Disclaimers

1. **Data source:** All shareholding data is sourced from KSEI's C-BEST system. Ownership percentages are calculated based on the total number of scripless shares recorded.
2. **Accuracy:** Some investor accounts may reflect nominee or custodian arrangements. Local/Foreign classification follows KSEI's own classification methodology.
3. **Conglomerate mapping:** Group assignments are based on holding company names in KSEI data and publicly available information. These may not capture all indirect or offshore ownership structures.
4. **Not investment advice:** This dashboard is a data visualisation tool for research and analytical purposes only. Nothing in this dashboard constitutes investment advice.
5. **Timeliness:** Named investor data reflects a snapshot date. Shareholding positions change continuously due to market transactions. January data shown in the Trend Tracker is aggregate (category-level) only, not individual named investors.

---

## 📄 License

This project is for analytical and educational purposes. Raw data belongs to KSEI. Please refer to [KSEI's terms of use](https://www.ksei.co.id) before redistribution.

---

*Built with ❤️ using Python + D3.js | Data: KSEI Jan–Mar 2026*
