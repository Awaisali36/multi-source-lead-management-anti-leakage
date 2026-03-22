# Multi-Source Lead Management & Anti-Leakage System
### Every Lead Captured. Every Field Validated. Zero Duplicates. Zero Leakage.

<div align="center">

![Type](https://img.shields.io/badge/Type-Delivered%20Client%20System-%233A7CFF?style=for-the-badge)
&nbsp;
![Leakage](https://img.shields.io/badge/Lead%20Leakage-Reduced%2060%25-%2300C853?style=for-the-badge)
&nbsp;
![Pipelines](https://img.shields.io/badge/Dual%20Pipeline-CRM%20%2B%20Nurture-%23FF6B35?style=for-the-badge)

</div>

<br/>

---

## What This Is

A dual-pipeline lead management system built for **Century 21 Alton Clark** — 8 automated workflows routing leads from 4 real estate prospecting sources into both Real Geeks CRM and Homebot simultaneously, with AI-powered data normalization, strict validation, and 15-minute deduplication windows that prevent duplicate entries before they happen.

Every lead that enters the system is clean, complete, correctly attributed, and in both platforms within seconds.

<br/>

---

## Why This Matters

> In real estate, a missed lead isn't just a missed sale — it's a lost relationship that someone else will build.

Real estate teams prospect across multiple sources simultaneously. Each source produces data in a different format. Someone enters it manually. Fields get missed. Names aren't split correctly. Addresses are inconsistent. The same lead appears twice because two team members found them independently.

```
  4 prospecting sources × average 50 leads/week each
= 200 leads per week entering the system

  Manual entry error rate: ~15%
= 30 leads per week with bad data

  Bad data leads that never get properly nurtured: ~60%
= 18 leads per week falling through the cracks

  Average real estate commission: $8,000–$15,000
  Even 1 recovered deal per month
= $96,000–$180,000 in protected annual revenue
```

Lead leakage isn't a data problem. It's a revenue problem.

<br/>

---

## Before vs. After

```
╔══════════════════════════════════════════════════════════════════╗
║                        BEFORE                                    ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  Lead found from Expired listing                                 ║
║       │                                                          ║
║       ▼                                                          ║
║  Manually entered into spreadsheet      ← inconsistent format    ║
║       │                                                          ║
║       ▼                                                          ║
║  Someone copies to Real Geeks CRM       ← full name, not split   ║
║       │                                                          ║
║       ▼                                                          ║
║  Someone separately adds to Homebot     ← maybe. maybe not.      ║
║       │                                                          ║
║       ▼                                                          ║
║  Duplicate found 3 days later           ← two agents working it  ║
║       │                                                          ║
║       ▼                                                          ║
║  Source not tracked                     ← no attribution data     ║
║                                                                  ║
║  RESULT: Inconsistent data · Missed nurture · Duplicate outreach ║
╚══════════════════════════════════════════════════════════════════╝


╔══════════════════════════════════════════════════════════════════╗
║                         AFTER                                    ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  Lead added to Google Sheet                                      ║
║       │                                                          ║
║       ├──► AI parses full name → first + last    ← instant       ║
║       ├──► Address standardized automatically    ← instant       ║
║       ├──► Phone + email validated               ← instant       ║
║       ├──► 15-min dedup window checked           ← instant       ║
║       ├──► Source auto-tagged (Expired/FSBO/etc) ← instant       ║
║       ├──► Pushed to Real Geeks CRM              ← instant       ║
║       └──► Pushed to Homebot nurture             ← instant       ║
║                                                                  ║
║  RESULT: Clean data · Both systems synced · Zero duplicates      ║
╚══════════════════════════════════════════════════════════════════╝
```

<br/>

---

## System Architecture

8 workflows — 2 per lead source, one for each destination platform:

```
┌─────────────────────────────────────────────────────────────────────┐
│                      GOOGLE SHEETS INTAKE                           │
│              (4 tabs — one per lead source)                         │
└──────┬─────────────┬──────────────┬──────────────┬──────────────────┘
       │             │              │              │
       ▼             ▼              ▼              ▼
   EXPIRED         FSBO        PRE-FORE-        PROBATE
   LISTINGS                    CLOSURE
       │             │              │              │
       └─────────────┴──────────────┴──────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    VALIDATION & NORMALIZATION                        │
│                                                                      │
│   AI name parsing      → "John Smith" → first: John / last: Smith   │
│   Address format       → "Street, City, State ZIP"                  │
│   Phone validation     → 10-digit, formatted consistently           │
│   Email validation     → required, format check                     │
│   Dedup window         → 15-minute check against recent entries      │
│   Source tagging       → auto-assigned from intake tab              │
│   Error flagging       → incomplete records returned with flag      │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                    ┌──────────┴──────────┐
                    ▼                     ▼
           REAL GEEKS CRM           HOMEBOT
           (Pipeline 1)             (Pipeline 2)
           Contact created          Contact enrolled
           Source attributed        Nurture sequence
           Lead score assigned      triggered
```

<br/>

---

## The 4 Lead Sources

| Source | What It Is | Why It Matters |
|---|---|---|
| **Expired Listings** | Properties that were listed but didn't sell | Sellers still motivated — often frustrated and ready to switch agents |
| **FSBO** | For Sale By Owner — selling without an agent | High-intent sellers who need help they don't know they need |
| **Pre-Foreclosure** | Homeowners behind on mortgage payments | Time-sensitive — motivated to sell before foreclosure damages credit |
| **Probate** | Properties in estate/inheritance situations | Heirs often need to liquidate quickly — low competition niche |

Each source has distinct data characteristics and nurture requirements. The system handles all four with source-specific attribution.

<br/>

---

## Data Validation Rules

| Field | Rule | On Failure |
|---|---|---|
| **Full Name** | AI splits into first + last | Flag if single word only |
| **Address** | Must match: `Street, City, State ZIP` | Flag for manual review |
| **Phone** | 10-digit, numeric only | Flag — required for outreach |
| **Email** | Valid format, not empty | Flag — required for Homebot |
| **Lead Source** | Auto-assigned from tab | Never missing |
| **Duplicate** | Not seen in last 15 minutes | Block entry, log as duplicate |

Records that fail validation are returned with an error flag in the Google Sheet — visible to the team for manual correction without stopping the pipeline.

<br/>

---

## Results

| Metric | Outcome |
|---|---|
| Lead leakage reduction | **~60%** |
| Manual CRM data entry eliminated | **100%** |
| Platforms synced per lead | **2 (Real Geeks + Homebot)** |
| Duplicate entries prevented | **15-minute dedup window** |
| Lead sources tracked | **4 (Expired · FSBO · Pre-Foreclosure · Probate)** |
| Data validation enforcement | **Every record, every field** |
| Hours saved on manual entry weekly | **Significant** |

<br/>

---

## Tech Stack

| Layer | Tool | Purpose |
|---|---|---|
| **Orchestration** | Zapier | 8-workflow automation pipeline |
| **Lead Intake** | Google Sheets | 4-tab source management |
| **AI Processing** | AI by Zapier | Name parsing and data normalization |
| **CRM** | Real Geeks | Primary lead management and tracking |
| **Nurture** | Homebot | Automated lead nurture sequences |
| **Validation** | Custom logic | Field enforcement and error flagging |
| **Deduplication** | 15-min window logic | Duplicate prevention |

<br/>

---

## Repository Structure

```
📁 multi-source-lead-management-anti-leakage/
├── 📄 README.md
├── 📁 workflow/
│   └── lead-management-workflows.md    ← Zapier workflow documentation
└── 📁 docs/
    └── validation-rules.md             ← Full validation and dedup logic
```

<br/>

---

## Built by Trilles AI

This system was designed and delivered by **[Awais Ali](https://www.linkedin.com/in/awais-ali-tillesai)**, CEO & Co-Founder of **[Trilles AI](https://www.trillesai.com)**.

If your real estate team is losing leads to bad data, manual entry errors, or systems that don't stay in sync — this is exactly what we fix.

<div align="center">

[![Connect on LinkedIn](https://img.shields.io/badge/Connect%20on%20LinkedIn-%230A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/awais-ali-tillesai)
&nbsp;
[![Visit Trilles AI](https://img.shields.io/badge/Visit%20Trilles%20AI-%233A7CFF?style=for-the-badge&logoColor=white)](https://www.trillesai.com)
&nbsp;
[![Email](https://img.shields.io/badge/Email%20Me-%23EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:letsautomatewithawais@gmail.com)

</div>

<br/>

---

<div align="center">
<sub>Built with precision · Powered by Trilles AI · <code>www.trillesai.com</code></sub>
</div>
