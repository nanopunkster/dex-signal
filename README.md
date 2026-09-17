Dexscreener Flip & Signals

Chrome/Brave extension for dexscreener.com. Flips the chart display, swaps candle colors, and runs a live EMA/RSI/MACD buy/sell signal engine — both on the active tab and in the background for a watch list.

Everything defaults OFF. Nothing is auto-applied from storage, and every toggle resets back to OFF whenever the chart swaps to a new pair or the page reloads — you never end up trading on a flipped view without knowing it.

Features
Live Signal panel (top-right, draggable, one combined box)

Chart controls and the live signal readout live in a single floating panel — no separate boxes to hunt for.

Grab the "Live Signal" header to drag the whole panel anywhere on screen. Position resets to the default corner on reload.
Cyberpunk terminal look: monospace, neon green/magenta glow, scanline overlay, pulsing confidence bar.
Live readout: pair, active timeframe, signal (BUY/SELL/—), confidence %, price, RSI14, EMA9, EMA21, MACD histogram, vote count.
Timeframe row reads Dexscreener's own toolbar and lets you switch resolution from the panel.
The moment a signal actually fires (same trigger as the sound alert), the panel does a gentle glow-pulse colored to match buy/sell, so you know which pair it was for even if you only heard the sound.
Chart Controls, in the same panel, each button showing an explicit ON/OFF state:
⇅ Price Flip — inverts the price scale via the chart's own native invert. Candles flip vertically, all numbers/labels stay fully readable.
🎨 Swap Colors — swaps candle up/down colors (body, border, wick).
☆ Watch Pair — adds the current pair to the background watch list so you get alerts even with the tab closed.
Background watch list (extension popup)
Paste a dexscreener.com pair URL to add it to the watch list.
Polled once a minute (chrome.alarms, MV3's minimum interval) via Dexscreener's public API.
Builds its own rolling price history (~5 hours) since the public API has no historical candles, only a live snapshot — this makes background signals an approximation of the real chart, not identical to it.
Alerts: OS notification, sound (via an offscreen document, since service workers can't use the Audio API directly), and on-page banner — each toggleable in the popup.
How it works
File	World	Role
injected.js	Page (MAIN)	Reads Dexscreener's real TradingView chart object, runs the live indicator poll, dispatches flip/color actions
indicators.js	Page (MAIN) + background	Shared EMA/RSI/MACD confluence logic
content.js	Isolated	Builds the combined Live Signal panel (readout + chart controls), plays sounds, shows the banner, relays watch-list requests
background.js	Service worker	Polls the watch list every minute, stores price history, fires OS notifications
popup.html / popup.js	Popup	Manage the watch list and alert settings
offscreen.html / offscreen.js	Offscreen doc	Plays alert sounds for the background-watch path

injected.js and content.js only talk to each other via CustomEvents on window — the standard bridge between a page-world and isolated-world content script. No shared JS objects.

injected.js dispatches the flip/color actions and drives the live indicator poll; content.js builds the panel and reacts.

Install (unpacked, for testing)
Download and unzip the extension folder.
Open chrome://extensions (or brave://extensions).
Turn on Developer mode (top right).
Click Load unpacked and select the dexscreener-flip-signals folder.
Open a pair on dexscreener.com — the Live Signal panel should appear top-right.
Notes
Signal logic is EMA9/21 crossover + confirmation — not financial advice, just a confluence indicator.
Background polling needs ~30 minutes (30 one-minute polls) before its first possible signal; the active-tab widget uses real chart candles so it's faster once ~30 bars are on the current timeframe.
All permissions are scoped to dexscreener.com and api.dexscreener.com only.
