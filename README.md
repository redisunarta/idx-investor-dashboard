# 🇮🇩 IDX Top Investor Dashboard

**An interactive web dashboard mapping significant shareholders (>1% ownership) across all IDX-listed companies, built from official KSEI (Kustodian Sentral Efek Indonesia) data.**

🔗 **[Live Dashboard →](https://your-username.github.io/idx-investor-dashboard/)**

---

## 📌 Overview

This dashboard provides a comprehensive view of institutional and individual investors who hold more than 1% of shares in any company listed on the Indonesia Stock Exchange (IDX/BEI). It enables analysts, researchers, and investors to:

- Identify who the major shareholders are across the entire IDX universe
- Understand how investors diversify across sectors and companies
- Map the ownership network between investors and stocks
- Track changes in significant shareholding over time

### Key Statistics (as of 31 March 2026)

| Metric | Value |
|---|---|
| IDX-listed stocks covered | **955 companies** |
| Unique investors with >1% holdings | **5,176 investors** |
| Total investor–stock positions tracked | **7,213 holdings** |
| Data source | KSEI Balance Position Report |
| Data date | 31 March 2026 |

---

## 🗂️ Data Sources

All data is sourced directly from **KSEI (Kustodian Sentral Efek Indonesia)**, the official central securities depository of Indonesia.

| File | Description |
|---|---|
| `Balancepos_Kepemilikan Efek.txt` | Daily balance position file — shareholding breakdown per stock by investor category (Local/Foreign × Institution type) |
| `StatisEfek_Master File Efek.txt` | Master security file — stock metadata including company name, sector, listing date, and closing price |
| `Pemegang Saham di atas 1% (KSEI).pdf` | KSEI's official report listing all named shareholders holding >1% of any IDX-listed company |

> Data files follow the pipe-delimited (`|`) format as published by KSEI's C-BEST system. The PDF report is processed using `pdfplumber` to extract named investor records.

---

## 🖥️ Dashboard Features

The dashboard is a **single self-contained HTML file** with embedded data — no backend or build step required. It uses [D3.js v7](https://d3js.org/) for all visualisations.

### Tab 1 — Overview
A high-level summary with four interactive charts:

- **Top 20 Investors by Number of Holdings** — horizontal bar chart ranking investors by the number of IDX companies they hold >1% in; hover to see total value and sector diversification
- **Top 15 Sectors by Investor Count** — shows which industries attract the most significant shareholders
- **Local vs Foreign Distribution** — donut chart of domestic versus foreign investor positions
- **Investor Type Distribution** — breakdown by entity type (Corporation, Individual, Mutual Fund, Pension Fund, Insurance, Securities Company, Foundation, Others); hover slices to expand

### Tab 2 — Top Investors
A sortable, filterable table of up to 500 significant investors:

- **Columns:** Investor Name · Stocks Held · Unique Sectors · Total Holding Value (IDR) · Investor Type · Local/Foreign
- **Filters:** Search by name, filter by investor type or Local/Foreign
- **Click any row** to expand the investor's full portfolio below the table — showing every stock they hold >1% in, with ownership percentage, holding value, and sector

### Tab 3 — All Holdings
A complete searchable table of all 7,213 individual investor–stock positions:

- Search simultaneously across stock code, company name, or investor name
- Filter by sector or Local/Foreign
- Sort by any column (%, shares, value, etc.)
- Paginated 50 rows at a time

### Tab 4 — Network Map
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
- **Click any node** to highlight its connections and dim the rest; click the canvas to reset

### Tab 5 — Sector Analysis
Sector-level breakdown with interactive charts:

- Horizontal bar chart of all sectors ranked by number of stocks with >1% shareholders
- **Hover** any bar to see investor count and total holding value
- **Click any bar or table row** to instantly generate a detailed stock list for that sector — showing all stocks, how many investors hold >1%, key investor names, and total holding value

---

## 🚀 Getting Started

### Option A — View Locally
Download `idx_investor_dashboard.html` and open it directly in any modern browser (Chrome, Firefox, Edge, Safari). No installation needed.

### Option B — Deploy to GitHub Pages
1. Fork or clone this repository
2. Rename `idx_investor_dashboard.html` → `index.html`
3. Go to **Settings → Pages → Source** and select `main` branch, root `/`
4. Your dashboard will be live at `https://your-username.github.io/idx-investor-dashboard/`

> The dashboard requires an internet connection only to load D3.js from CDN (`cdnjs.cloudflare.com`). All data is embedded in the HTML file.

---

## 🔄 Updating the Data

To refresh with new KSEI data:

1. Download the latest files from the KSEI website:
   - Balance Position (`Balancepos_*.txt`)
   - Master Security File (`StatisEfek_*.txt`)
   - Shareholder PDF (`Pemegang Saham di atas 1%_*.pdf`)

2. Run the extraction scripts:
```bash
pip install pdfplumber pandas
python extract_pdf.py       # Extracts investor data from the PDF
python prepare_data.py      # Merges with master file, builds JSON
python build_dashboard.py   # Generates the dashboard HTML
```

3. The output `idx_investor_dashboard.html` is ready to deploy.

---

## 📐 Technical Notes

| Item | Detail |
|---|---|
| Frontend | Vanilla HTML + CSS + JavaScript |
| Visualisation library | D3.js v7.8.5 (CDN) |
| Data format | JSON embedded in HTML (~1.9 MB total file) |
| Network layout | D3 Force Simulation with collision, charge, and link forces |
| PDF extraction | `pdfplumber` Python library |
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

---

## ⚠️ Disclaimers

1. **Data source:** All shareholding data is sourced from KSEI's C-BEST system. Ownership percentages are calculated based on the total number of scripless shares recorded.
2. **Accuracy:** Some investor accounts may reflect nominee or custodian arrangements. Local/Foreign classification follows KSEI's own classification methodology.
3. **Not investment advice:** This dashboard is a data visualisation tool for research and analytical purposes only. Nothing in this dashboard constitutes investment advice.
4. **Timeliness:** Data reflects a single snapshot date. Shareholding positions change continuously due to market transactions.

---

## 📄 License

This project is for analytical and educational purposes. Raw data belongs to KSEI. Please refer to [KSEI's terms of use](https://www.ksei.co.id) before redistribution.

---

*Built with ❤️ using Python + D3.js | Data: KSEI 31 March 2026*
