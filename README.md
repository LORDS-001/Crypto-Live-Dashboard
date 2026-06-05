CRYPTOPULSE
Real-Time Market Intelligence Terminal · v3.7.2

A zero-dependency, single-file crypto trading analytics dashboard built with vanilla HTML, CSS, and JavaScript. No backend. No build step. Just open and run.


Preview
╔══════════════════════════════════════════════════════╗
║  CRYPTOPULSE  // REAL-TIME MARKET INTELLIGENCE v3.7.2 ║
║  ● LIVE FEED   12:31:11 UTC   NETWORK: MAINNET        ║
╠══════════════════════════════════════════════════════╣
║  $2.74T      $98.6B       52.8%        73 / GREED     ║
║  Market Cap  24H Volume   BTC Dom.     Fear & Greed   ║
╠══════════╦═══════════════════════════════════════════╣
║ TOP 10   ║  BITCOIN // BTC  $67,432.44  ▲ +2.84% 7D  ║
║ ASSETS   ║  [Multi-line animated price chart]         ║
╠══════════╩═══════════════════════════════════════════╣
║  Market Cap Distribution  ║  24H Trading Volume       ║
║  [Horizontal bar chart]   ║  [Vertical bar chart]     ║
╚══════════════════════════════════════════════════════╝

Features
Market Overview

Global Metrics Bar — Live total market cap, 24H volume, BTC dominance, and Fear & Greed index displayed with delta indicators
Live Ticker Strip — Horizontally scrolling price feed for all 10 tracked assets with color-coded percentage changes
UTC Clock — Real-time UTC clock updated every second in the header

Asset Tracking

Top 10 Assets Table — Ranked list with coin icon, name, ticker symbol, current price, and 24H percentage change
Interactive Row Selection — Click any asset row to load its price chart instantly
Live Price Simulation — All prices flicker with simulated market drift every 3 seconds

Price Chart

Canvas-Rendered Line Chart — Smooth animated draw-on entrance for the main price line
3 Overlaid Series — Primary price line (green), and two moving average overlays (red, blue)
Gradient Area Fill — Translucent fill beneath the main price line for visual depth
Glow Effect — CSS shadow blur on the primary series for the terminal aesthetic
Hover Tooltip — Crosshair tooltip showing time label and exact price at cursor position
Timeframe Switcher — Toggle between 24H, 7D, 1M, and 1Y views; chart data regenerates per coin per timeframe
Y-Axis Price Labels — Formatted price gridlines with K-shorthand for large values
X-Axis Time Labels — Context-aware time labels that adapt to the selected timeframe

Analytics Charts

Market Cap Distribution — Horizontal bar chart for the top 8 assets with animated fill-in on load
24H Trading Volume — Vertical grouped bar chart with gradient fill and entrance animation
Grid Lines — Subtle reference lines on both charts with labeled value axes


Design System
Color Palette
TokenHexUsage--bg#020f09Page background--bg2#041a0eHeader / ticker background--panel#071a0dCard / panel background--border#0d3a1cPanel borders--border2#0f4a22Corner accents / bar borders--accent#00e676Primary neon green--accent2#00c853Gradient end for bars--red#ff1744Negative price changes--blue#2979ffSecondary chart line--text#b2dfdbBody text--text-dim#2e7d6bLabels, secondary info
Typography
FontWeightUsageOrbitron400/700/900Logo, coin names, large valuesShare Tech Mono400All data readouts, labels, pricesRajdhani400–700Body text, asset names
Visual Details

CRT Scanline Overlay — Full-page repeating gradient simulating a terminal screen texture
Animated Scan Line — A single translucent line sweeps down each chart panel on loop
Panel Corner Brackets — CSS-drawn ┌ ┐ └ ┘ corner decorations on every card
Top Edge Gradient — Subtle left-to-right accent gradient on the top border of each panel
Pulsing Live Dot — Animated scale+opacity pulse on the live feed indicator
Custom Scrollbar — Slim 4px scrollbar styled to match the dark theme


Project Structure
cryptopulse.html        # Entire application — single self-contained file
README.md               # This file
Everything lives in cryptopulse.html:
cryptopulse.html
├── <head>
│   ├── Google Fonts import (Orbitron, Share Tech Mono, Rajdhani)
│   └── <style>  — CSS variables, layout, components, animations
└── <body>
    ├── <header>             — Logo, live badge, UTC clock, network status
    ├── .ticker-strip        — Scrolling price ticker
    ├── .metrics-bar         — 4 global market metric cards
    ├── .main-grid
    │   ├── #assetList       — Top 10 ranked asset rows
    │   └── #mainChart       — Canvas price chart with tooltip
    ├── .bottom-grid
    │   ├── #mcapBars        — Horizontal market cap bars
    │   └── #volChart        — Canvas volume bar chart
    ├── <footer>             — Terminal status line
    └── <script>             — All JS: data, rendering, interactions, animations

Getting Started
No installation, no dependencies, no build tools required.
bash# Option 1 — just open it
open cryptopulse.html

# Option 2 — serve locally (avoids font CORS on some systems)
npx serve .
# or
python3 -m http.server 8080
Then navigate to http://localhost:8080/cryptopulse.html.

Note: An internet connection is required on first load to fetch fonts from Google Fonts. All other functionality runs entirely offline.


Data Model
All market data is simulated. No API keys or network requests are made for prices.
Coin Schema
js{
  id:    'BTC',         // Internal identifier
  name:  'Bitcoin',     // Display name
  sym:   'BTC',         // Ticker symbol
  price: 67420.32,      // Current price (USD)
  chg:   2.84,          // 24H percentage change
  mcap:  1331,          // Market cap (USD billions)
  vol:   40.2,          // 24H volume (USD billions)
  color: '#f7931a',     // Brand color for icon
  abbr:  'BT',          // 2-char icon label
}
Chart Data Generation
Price history is generated via a seeded pseudo-random walk — the same coin + timeframe always produces the same chart shape, preventing jarring redraws on repeated selections.
js// Seed is derived from coin ID + timeframe string
const seed = coin.id + tf;   // e.g. "BTC7D"
// Deterministic LCG: s = (s * 1664525 + 1013904223) & 0xffffffff
Volatility scales with timeframe:
TimeframePointsVolatility per Step24H480.8%7D841.5%1M902.5%1Y1204.0%

Interactions
ActionResultClick asset rowLoads that coin's chart and updates headerClick 24H / 7D / 1M / 1YRegenerates chart for the selected timeframeHover over price chartShows tooltip with time label and priceWait 3 secondsAll prices drift slightly (live simulation)Resize windowCharts re-render at new dimensions

Browser Support
Requires a modern browser with Canvas 2D API support.
BrowserSupportChrome 90+✅Firefox 88+✅Safari 14+✅Edge 90+✅IE 11❌

Extending the Project
Add real data — Replace the COINS array population with a fetch to a public API such as CoinGecko:
jsconst res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=10');
const data = await res.json();
Add more coins — Push additional entries to the COINS array following the schema above.
Persist selected coin — Store selectedCoin.id in localStorage and restore it on page load.
Add candlestick chart — Replace the line chart renderer with OHLC bar logic using the same canvas setup.
Dark/light toggle — Swap CSS variable values on document.documentElement at runtime.

License
This project is released for demonstration purposes. All price data is entirely simulated and should not be used for actual trading decisions.

// CRYPTOPULSE TERMINAL · DATA SIMULATED FOR DEMONSTRATION · ENCRYPTED CHANNEL ESTABLISHED //