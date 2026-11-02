# 🐙 Open Octopus

> **Unofficial** open-source toolkit for [Octopus Energy](https://octopus.energy) customers.
> Not affiliated with or endorsed by Octopus Energy.

<img src="docs/menubar-screenshot.png" width="300" alt="Open Octopus Menu Bar App">

## What's Included

| Component | Description |
|-----------|-------------|
| **Menu Bar App** | Native macOS app with live rates, usage, insights |
| **CLI Tools** | Terminal commands for quick access to your data |
| **TUI** | Interactive terminal dashboard |
| **AI Assistant** | Ask questions about your energy usage |
| **Alerts** | Notifications for rate changes, charging, sessions *(coming soon)* |

## Features

### Menu Bar App (macOS)
- Live rate display with countdown to off-peak
- Smart charging status with golden indicator when EV is charging
- Usage sparkline with off-peak hour highlighting
- Rate comparison (peak vs off-peak savings)
- Monthly cost projection
- AI chat with quick action buttons

### CLI Tools
```bash
octopus rate       # Current electricity rate
octopus account    # Account balance
octopus usage      # Consumption data
octopus dispatch   # EV charging schedule
octopus power      # Live power (if available)
octopus sessions   # Saving sessions
```

### TUI (Terminal UI)
```bash
octopus tui        # Interactive dashboard
```

### AI Assistant
```bash
octopus-ask "What's the best time to run my dishwasher?"
octopus-ask "How much did I spend this week?"
octopus-ask "Compare my usage to last month"
```

## Installation

```bash
pip install open-octopus
```

### Platform notes

- The **Python API client + CLI/TUI** are cross-platform (macOS, Linux, Windows).
- The **Menu Bar App** is macOS-only.

### macOS Menu Bar prerequisites (optional)

If you want the Python menu bar integration (\`open_octopus.menubar\`), install the extra:

```bash
pip install open-octopus[menubar]
```

This requires:
- macOS
- Xcode Command Line Tools: \`xcode-select --install\`

On Windows/Linux, importing \`open_octopus\` will not fail; \`OctopusMenuBar\` is simply unavailable.

### Configuration

Set your credentials:
```bash
export OCTOPUS_API_KEY="sk_live_xxxxx"
export OCTOPUS_ACCOUNT="A-XXXXXXXX"
export OCTOPUS_MPAN="1234567890123"        # For consumption data
export OCTOPUS_METER_SERIAL="12A3456789"   # For consumption data
export ANTHROPIC_API_KEY="sk-ant-xxxxx"    # For AI features
```

Or create `~/.octopus.env`:
```bash
OCTOPUS_API_KEY=sk_live_xxxxx
OCTOPUS_ACCOUNT=A-XXXXXXXX
OCTOPUS_MPAN=1234567890123
OCTOPUS_METER_SERIAL=12A3456789
ANTHROPIC_API_KEY=sk-ant-xxxxx
```

### Menu Bar App (macOS)

The app needs full Xcode (not just Command Line Tools) and gets its data from the `octopus-server` helper, which it looks for in `~/.local/bin`. Credentials are read from `~/.octopus.env`.

```bash
git clone https://github.com/abracadabra50/open-octopus.git
cd open-octopus

# Install the Python helper (puts octopus-server in ~/.local/bin)
uv tool install ".[agent]"        # or: pipx install ".[agent]"

# First-time Xcode setup, if not already done
sudo xcodebuild -license accept
xcodebuild -runFirstLaunch

xcodebuild -workspace OctopusMenuBar.xcworkspace -scheme OctopusMenuBar \
  -configuration Release -derivedDataPath build build
cp -R build/Build/Products/Release/OctopusMenuBar.app /Applications/
open /Applications/OctopusMenuBar.app
```

## Recent changes

**Gas support**
- The menu bar now shows gas: unit rate, standing charge, daily use and cost, a half-hourly chart and a monthly projection.
- The menu bar title shows both rates, e.g. `⚡ 28.7p · 🔥9.3p`.
- Electricity and gas have their own bordered sections, under a header with the tariff name and the date a fixed price ends.

**Fixes**
- **Gas tariff lookup failed** with `400 Bad Request` (wrong GraphQL fragment for `GasTariffType`).
- **One unsupported feature blanked the whole app.** Accounts without an EV charger, Home Mini or saving sessions got an error screen. Each feature now fails on its own.
- **Flat-rate tariffs were treated as Intelligent Octopus Go.** Fixed tariffs got a made-up 7p off-peak rate, a countdown to it, and costs that were too low. They now show a single rate, and the CLI prints `FLAT RATE`.
- **Usage chart labels were wrong.** They now run 00:00–24:00 for the latest day of meter data.
- **The Xcode build failed** because `Config/*.xcconfig` was missing from the repo; those files are now included.

## Supported Tariffs

- Intelligent Octopus Go
- Octopus Go
- Agile Octopus
- Flexible Octopus
- Tracker
- And more...

## Roadmap

- [ ] **Alerts** - macOS notifications for:
  - Off-peak rate starting/ending
  - EV dispatch starting/ending
  - Saving sessions
  - Low balance warning
- [ ] **Widgets** - macOS desktop widgets
- [ ] **Gas support** - Full gas meter integration
- [ ] **Historical charts** - Weekly/monthly usage graphs

## License

MIT

## Credits

Built with Python and SwiftUI. AI powered by [Claude](https://anthropic.com).
