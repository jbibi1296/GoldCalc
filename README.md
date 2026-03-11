# GoldFix GF-1 Pro

A single-file web app for calculating gold jewelry prices — built to look and feel like a physical calculator. Works on desktop and mobile, can be saved to your iPhone or Android home screen as a full-screen app.

---

## Features

### Calculator Modes
| Mode | What it does |
|------|-------------|
| **BUY** | Gold value × buy multiplier (default 0.95) |
| **SELL** | (Gold value + rates + fees) × sell multiplier (default 3) |
| **COST** | Gold value + rates + fees, no multiplier |

### Inputs
- **Spot price** — fetched live from Kitco on load, or enter manually via the numpad using the `SPOT` button
- **Weight** — entered via numpad in **DWT** or **GRM**
- **Karat** — 10K, 14K, or 18K

### Settings
- **Multipliers** — customize the buy and sell multipliers
- **Rates** — per-DWT charges (e.g. Labor, Casting). Each rate has a name, dollar amount per DWT, and scope (Selling / Cost / Both). Fully editable and deletable.
- **Fees** — flat dollar fees (e.g. Printing Fee, Design Fee). Same add/edit/delete controls.

### Receipt Tape
When you hit **CALCULATE**, a receipt tape slides down from the top of the calculator showing:
- Spot price and price per DWT for the selected karat
- Full line-item breakdown (gold value, rates, fees, subtotal)
- Final price in large bold type
- Date and time of the calculation
- Tap **✕** to dismiss

### Live Spot Price
- Auto-fetches from Kitco on load via two CORS proxy fallbacks
- Shows live/offline badge and last-fetched timestamp
- Hit **↻ REFRESH** to re-fetch at any time
- Press **SPOT** on the calculator to manually enter a price via numpad

---

## Usage

### Numpad Target
The numpad can write to either the **spot price** or the **weight**. Toggle between them using the `SPOT` / `WGHT` buttons above the numpad. On desktop, **Tab** also toggles.

### Keyboard shortcuts (desktop)
| Key | Action |
|-----|--------|
| `0–9`, `.` | Enter digits |
| `Backspace` | Delete last digit |
| `Delete` | Clear all |
| `Enter` | Calculate |
| `Tab` | Toggle SPOT / WGHT input |

---

## Formulas

**Price per DWT** (shown on tape):
```
pricePerDWT = (spotPrice / 31.1035) × (karat / 24) × 1.55517
```

**Buy price:**
```
goldValue = (weightGrams × purity × spotPrice) / 31.1035
buyPrice  = goldValue × buyMultiplier
```

**Sell price:**
```
ratesTotal = sum of (rate.amount × weightDWT) for applicable rates
feesTotal  = sum of applicable flat fees
subtotal   = goldValue + ratesTotal + feesTotal
sellPrice  = subtotal × sellMultiplier
```

**Cost price:**
```
costPrice = goldValue + ratesTotal + feesTotal
```

Conversions used:
- 1 troy oz = 31.1035 grams
- 1 DWT = 1.55517 grams

---

## Deployment

### Netlify Drop (easiest — no account needed)
1. Rename `gold-calculator-v5-mobile.html` → `index.html`
2. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
3. Drag and drop the file
4. Done — you get a live `https://` URL instantly

### GitHub Pages (free, permanent)
1. Create a repo at [github.com](https://github.com)
2. Upload `index.html`
3. Go to **Settings → Pages**, set source to `main` branch
4. Your URL: `https://yourusername.github.io/reponame`

### Custom domain
Both Netlify and GitHub Pages support custom domains. Buy a domain from Namecheap or Google Domains, then follow the provider's DNS setup guide.

---

## Mobile / PWA

The app is fully responsive for portrait and landscape phones. It includes the necessary meta tags to run as a full-screen progressive web app:

**iPhone:** Open in Safari → Share → **Add to Home Screen**  
**Android:** Open in Chrome → menu → **Add to Home Screen**

Once installed it opens with no browser chrome, just like a native app.

---

## Tech

- Pure HTML/CSS/JS — no frameworks, no build step, no dependencies
- Single `.html` file — everything is self-contained
- Live gold price via [Kitco](https://www.kitco.com) API (CORS proxied)
- Fonts: [Barlow Condensed](https://fonts.google.com/specimen/Barlow+Condensed) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) via Google Fonts
- Sound: Web Audio API for the print sound effect

---

## File

| File | Description |
|------|-------------|
| `gold-calculator-v5-mobile.html` | The full app — rename to `index.html` to deploy |
