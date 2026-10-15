# Export Currency Tool

A single-page web app for Indian exporters to track foreign-currency invoices, compare spot rates against bank conversion rates, and calculate net gain or loss after CC interest costs.

**Live site:** deployed via GitHub Pages

---

## Features

### Invoice Management
- Log invoices with invoice number, customer, currency (USD / EUR / GBP), amount, received date, and spot rate
- Enter the bank rate and conversion date once the bank processes the payment
- Leave the bank rate blank to keep an invoice **Pending** and see the break-even rate you need from the bank

### Calculations
- **(A) Spot conversion** — amount × spot rate at receipt
- **(B) Bank conversion** — amount × bank rate
- **CC interest cost** — configurable % p.a. accrued over the holding period (days)
- **Break-even rate** — minimum bank rate to cover CC cost (spot × (1 + CC% × N / 365))
- **Net gain / loss** — (B − A) minus CC interest cost
- **What-if table** — shows the break-even rate at 0, 1, 2, 3, 5, 7, and 10 days after receipt

### Live Exchange Rates
Rates are fetched automatically and applied to the spot rate field. Sources are tried in order:

| Source | Used when |
|--------|-----------|
| Alpha Vantage | API key configured; refreshes every 60 min |
| open.er-api.com | Default fallback (every 5 min) |
| fawazahmed0 CDN | Secondary fallback |
| frankfurter.dev | Tertiary fallback |

### Reports
Filter by month and see breakdowns by month, customer, currency, and bank — including a bank-wise average premium over spot (useful for negotiation).

### Rate Trend & Alerts
- 30-day line chart (business days) via frankfurter.dev
- Set an alert when a currency reaches a target INR rate (above or below); browser notifications supported

### Data & Sync
- **Firebase Firestore** — real-time sync across sessions (all users sharing the app see the same data instantly)
- **localStorage** — automatic fallback if Firestore is unavailable
- **Export CSV** — download filtered invoice list
- **Backup JSON / Import** — full offline backup and restore

---

## Getting Started

The app runs entirely in the browser with no build step.

```bash
# Clone the repo
git clone https://github.com/<your-org>/Export-Realization.git
cd Export-Realization

# Open locally — no server required
open index.html
```

For live exchange rates beyond the free fallbacks, get a free key at [alphavantage.co](https://www.alphavantage.co/support/#api-key) and paste it into **Rate Source Settings** inside the app. The key is stored in localStorage and is never sent anywhere except Alpha Vantage.

---

## Deployment

Push to `main` — GitHub Actions automatically deploys to GitHub Pages.

```
.github/workflows/deploy.yml  ← triggers on push to main
```

---

## Project Structure

```
index.html    ← entire app (HTML + CSS + JS, single file)
.github/
  workflows/
    deploy.yml
```

---

## Formula Reference

| Symbol | Meaning |
|--------|---------|
| A | Amount × Spot rate |
| B | Amount × Bank rate |
| N | Days between received date and bank conversion date |
| CC cost | A × (CC% / 100) / 365 × N |
| Break-even | Spot × (1 + CC% / 100 × N / 365) |
| Net | (B − A) − CC cost |

---

## Tech Stack

- Vanilla HTML / CSS / JavaScript (no framework, no bundler)
- [Firebase JS SDK v10](https://firebase.google.com/) — Firestore real-time database
- [Inter](https://fonts.google.com/specimen/Inter) via Google Fonts
- GitHub Pages for hosting
