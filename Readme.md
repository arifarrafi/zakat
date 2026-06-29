# ☪️ Zakat Tracker — Personal Zakat Calculation & Management

A complete, modern web application for calculating and tracking your annual Zakat obligation.
Built on **Google Sheets + Apps Script** (backend) and **GitHub Pages** (frontend) — no server needed.

---

## 📋 Table of Contents

1. [Features](#features)
2. [Architecture](#architecture)
3. [Backend Setup (Google Apps Script)](#backend-setup)
4. [Frontend Setup (GitHub Pages)](#frontend-setup)
5. [First-Run Configuration](#first-run-configuration)
6. [User Guide](#user-guide)
7. [Zakat Calculation Rules](#zakat-calculation-rules)
8. [Troubleshooting](#troubleshooting)

---

## ✨ Features

### Wealth Tracking
| Category | Description |
|---|---|
| 🏅 **Gold** | Items with karat, weight, market value, and selling (Zakat) value |
| 🥈 **Silver** | Items with karat, weight, market value, and selling value |
| 🏦 **Deposits** | Bank savings, cash, and other deposited money |
| 📈 **Capital Investments** | Investments where profit reinvests into capital (Zakatable) |
| 💹 **Monthly Investments** | Investments where profit is taken monthly (principal optional) |
| 📤 **Loans Receivable** | Money owed to you (adds to Zakatable wealth) |
| 📥 **Loans Payable** | Money you owe (deducted from Zakatable wealth) |

### Calculation Engine
- ✅ Configurable **Nisab** — 52.5 Tola Silver or 7.5 Tola Gold, any Karat (22/21/18/Traditional)
- ✅ **Custom Timeframe** — rate auto-adjusts: `(2.5% × days) ÷ 354`
- ✅ **Gold/Silver selling value** configurable as % of market price (default 80%)
- ✅ All rules individually toggle-able from the Settings tab
- ✅ Live market prices from **Bajus.org** (Bangladesh Jewellers' Somity) or manual entry

### Dashboard & History
- 📊 Real-time Zakat summary (due, paid, remaining)
- 📅 Hijri + Gregorian date display
- 📉 Nisab threshold progress bar
- 💰 Per-category wealth breakdown
- 🕐 Recent payment history
- 📋 Lifetime Zakat history across all timeframes

---

## 🏗 Architecture

```
┌──────────────────────────────────┐     HTTPS      ┌──────────────────────────────────┐
│         GitHub Pages             │ ─────────────► │     Google Apps Script           │
│         index.html               │ ◄───────────── │     (Web App / exec URL)         │
│  (All HTML + CSS + JavaScript)   │      JSON       │     Code.gs                      │
└──────────────────────────────────┘                 └──────────────┬───────────────────┘
                                                                    │ Sheets API
                                                     ┌──────────────▼───────────────────┐
                                                     │       Google Spreadsheet         │
                                                     │  Gold | Silver | Deposits |      │
                                                     │  Investments | Loans |           │
                                                     │  Market_Values | Rules |         │
                                                     │  Zakat_Timeframes | Payments     │
                                                     └──────────────────────────────────┘
```

**Sheets created automatically:**

| Sheet Name | Purpose |
|---|---|
| `Gold` | Gold jewellery/coin entries |
| `Silver` | Silver jewellery/coin entries |
| `Deposited_Money` | Bank accounts, cash |
| `Invested_Capital` | Profit-to-capital investments |
| `Invested_Monthly` | Monthly-profit investments |
| `Loan_Receivable` | Loans you've given out |
| `Loan_Payable` | Loans you've taken |
| `Market_Values` | Gold/Silver prices per gram (BDT) |
| `Zakat_Timeframes` | Yearly Zakat periods |
| `Zakat_Payments` | Payment records |
| `Rules` | All configuration settings |

---

## 🔧 Backend Setup

### Step 1 — Create Google Spreadsheet
1. Go to [sheets.google.com](https://sheets.google.com) → **+ Blank**
2. Name it: **"Zakat Tracker"** (or any name)

### Step 2 — Open Apps Script
1. In the spreadsheet: **Extensions → Apps Script**
2. A new script editor tab opens

### Step 3 — Paste the Backend Code
1. Delete all existing code in the editor
2. Open `Code.gs` from this repository
3. Copy the entire contents and paste into the editor
4. Press **Ctrl+S** (or Cmd+S on Mac) to save
5. Name the project: **"Zakat Tracker API"**

### Step 4 — Run Initial Setup
1. In the editor, make sure `setupAll` is selected in the function dropdown
2. Click **▶ Run**
3. When prompted, click **Review permissions** → **Advanced** → **Go to Zakat Tracker API (unsafe)** → **Allow**
4. Wait for it to complete — you'll see "Execution completed" in the log
5. Your Google Spreadsheet now has all 11 sheets created automatically ✓

### Step 5 — Deploy as Web App
1. Click **Deploy → New deployment**
2. Click the ⚙️ gear icon next to "Select type" → choose **Web app**
3. Fill in:
   - **Description**: `Zakat Tracker v1`
   - **Execute as**: `Me` (your Google account)
   - **Who has access**: `Anyone`
4. Click **Deploy**
5. **Copy the Web App URL** — it looks like:
   ```
   https://script.google.com/macros/s/AKfycb.../exec
   ```
6. Keep this URL safe — you'll need it to connect the frontend

> ⚠️ **Important**: Every time you edit `Code.gs`, you must create a **New Deployment** (not "Manage deployments → Edit"). The URL stays the same only if you redeploy to the same deployment ID.

---

## 🌐 Frontend Setup (GitHub Pages)

### Option A — Quick Start (GitHub)
1. Fork or create a new GitHub repository
2. Upload `index.html` to the repository root
3. Upload `.nojekyll` (empty file) to prevent Jekyll processing
4. Go to **Settings → Pages**
5. Under **Source**, select `Deploy from a branch`
6. Choose branch: `main` (or `master`), folder: `/ (root)`
7. Click **Save**
8. Your app will be live at: `https://yourusername.github.io/repo-name/`

### Option B — Local Use
Just open `index.html` directly in any modern browser. No server needed.

### Option C — Any Static Host
Upload `index.html` to Netlify, Vercel, Cloudflare Pages, or any static hosting.

---

## 🚀 First-Run Configuration

1. Open the app URL in your browser
2. A **setup screen** appears asking for the Google Apps Script URL
3. Paste the Web App URL from Step 5 above
4. Click **⚡ Connect & Initialize**
5. The app connects, verifies the sheets, and loads your dashboard

> The URL is saved in `localStorage` — you only need to enter it once per browser.

---

## 📖 User Guide

### Adding Gold / Silver
1. Go to **Gold** or **Silver** in the sidebar
2. Click **+ Add Gold Item**
3. Enter: Item Name, Karat, Weight (grams)
4. **Market Value** and **Selling Value** auto-fill from current market prices
5. You can manually override these values
6. Click **Save**

> 💡 **Tip**: Update market prices in **Market Values** first. Then use **♻ Recalculate Values** to update all existing entries.

### Setting Up a Zakat Timeframe
1. Go to **Timeframes** → **+ New Timeframe**
2. Enter a label (e.g., `12 Ramadan 1447 – 12 Ramadan 1448`)
3. Set Start Date and End Date
4. The app calculates days and adjusts the rate:
   - **Standard year** (354 days) → Rate = 2.5%
   - **Custom period** → Rate = (2.5 × days) ÷ 354
5. Set Status to **Active**
6. Click **Create Timeframe**

### Recording a Payment
1. Go to **Payments** → **+ Record Payment**
2. Select date and enter amount (৳)
3. Choose the associated Timeframe
4. The paid amount updates automatically in the Timeframe record

### Updating Market Prices
1. Go to **Market Values**
2. Click **🌐 Fetch from Bajus.org** to auto-update (requires GAS network access)
3. Or manually enter prices in the input fields and click **Save**
4. After updating prices, click **♻ Recalculate Assets** to refresh all Gold/Silver values

### Settings Reference

| Setting | Default | Description |
|---|---|---|
| **Nisab Metal** | Silver | Use Silver (52.5 Tola) or Gold (7.5 Tola) for Nisab |
| **Nisab Karat** | 22K | Which karat price to use for Nisab calculation |
| **Base Zakat Rate** | 2.5% | Standard Islamic Zakat rate |
| **Selling Price %** | 80% | Gold/Silver Zakat value as % of current market price |
| **Include Gold** | ✅ | Whether gold counts toward Zakatable wealth |
| **Include Silver** | ✅ | Whether silver counts toward Zakatable wealth |
| **Include Deposits** | ✅ | Whether bank/cash deposits count |
| **Include Capital Investments** | ✅ | Profit-reinvested investments |
| **Include Monthly Investments** | ❌ | Principal of monthly-profit investments |
| **Include Loan Receivable** | ✅ | Money owed to you |
| **Deduct Loan Payable** | ✅ | Money you owe (reduces Zakat wealth) |

---

## 📐 Zakat Calculation Rules

### Nisab Threshold
```
1 Tola = 11.664 grams

Silver Nisab = 52.5 Tola × 11.664 g/Tola = 612.36 g of silver
Gold Nisab   = 7.5  Tola × 11.664 g/Tola = 87.48  g of gold

Nisab Value (৳) = Weight (g) × Market Price (৳/g) for selected karat
```

### Zakat Rate for Custom Timeframes
```
Standard Rate = 2.5% (for 354-day lunar year)

Custom Rate   = (2.5% × Number of Days) ÷ 354

Example: 24 Oct 2024 – 20 Mar 2026 = 513 days
Custom Rate = (2.5 × 513) ÷ 354 = 3.6229%
```

### Zakatable Wealth Formula
```
Total Zakatable Wealth =
  + Gold Selling Value       (if included)
  + Silver Selling Value     (if included)
  + Deposited Money          (if included)
  + Capital Investments      (if included)
  + Monthly Investments      (if included, default OFF)
  + Loans Receivable         (if included)
  − Loans Payable            (if deduction enabled)

Selling Value = Weight × Market Price × (Selling % ÷ 100)
```

### Zakat Due
```
If Total Zakatable Wealth ≥ Nisab Value:
    Zakat Due = Total Zakatable Wealth × Effective Rate ÷ 100
Else:
    Zakat Due = 0
```

---

## 🔧 Troubleshooting

### "Failed to load data" error
- Verify your GAS Web App URL is correct (ends with `/exec`)
- Make sure deployment is set to **Who has access: Anyone**
- Try creating a new deployment if you edited the code

### CORS errors in browser console
- Ensure the GAS app is deployed as **Web App** (not API executable)
- Check that **Execute as: Me** and **Who has access: Anyone** are set
- Try in a different browser or incognito mode

### Market prices not fetching from Bajus.org
- Bajus.org may have changed their website structure
- Enter prices manually in Market Values
- The GAS script tries multiple Bajus.org URLs automatically

### Gold/Silver values not auto-calculating
- Go to **Market Values** and enter current prices first
- In Gold/Silver forms, change the Karat dropdown to trigger recalculation
- Use **♻ Recalculate Values** button to update all existing entries

### Prices showing ৳ 0.00
- Market prices haven't been set yet — go to Market Values first
- Nisab will also show ৳ 0 until prices are entered

### "Sheet not found" error
- Run `setupAll()` again from Apps Script editor
- Make sure you're running it in the correct spreadsheet

### After code changes, old behavior persists
- In Apps Script: **Deploy → New deployment** (don't edit existing)
- Clear browser cache or use Ctrl+Shift+R

---

## 📁 File Structure

```
zakat-tracker/
├── index.html       ← Complete frontend (single file)
├── Code.gs          ← Google Apps Script backend
├── .nojekyll        ← Prevents GitHub Pages Jekyll processing
└── README.md        ← This file
```

---

## 🔐 Privacy & Security

- All your data is stored in **your own Google Spreadsheet** — Anthropic/Google has no special access
- The GAS Web App executes as **your Google account**
- The GAS URL in `localStorage` is only accessible to you in your browser
- No ads, no tracking, no third-party analytics

---

## 📜 Islamic Finance Notes

> *"Take from their wealth a charity by which you purify them and cause them increase."*
> — Quran 9:103

- Zakat is due when wealth exceeds Nisab **and** has been held for one full lunar year (Hawl)
- The standard rate is **2.5%** of total Zakatable wealth
- Gold/Silver are valued at **selling price** (not purchase price)
- Debts owed to you (receivable) **increase** your Zakatable wealth
- Debts you owe (payable) **decrease** your Zakatable wealth
- Monthly-profit investment **principals** are generally not Zakatable (profit is already being consumed)

---

*Built with ☪️ for the Muslim community. May Allah accept your Zakat.*
