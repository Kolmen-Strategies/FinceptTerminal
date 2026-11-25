# Fincept Terminal — Kolmen fork

A C++20/Qt6 financial desktop application, forked from [Fincept-Corporation/FinceptTerminal](https://github.com/Fincept-Corporation/FinceptTerminal). This snapshot declares version **4.0.2** in `fincept-qt/CMakeLists.txt`. It includes native screens, HTTP/WebSocket clients, local storage, Python analytics scripts and optional external data/LLM integrations.

Python analytics run through managed interpreter/subprocess infrastructure (`fincept-qt/src/python/`), with scripts/resources deployed alongside the application. “Single native binary” does not mean a dependency-free or fully offline product. Some calculations, including the QuantLib screen, use hosted Fincept APIs whose server code is outside this repository.

## Feature verification — 2026-11-25

Audited source: `8bf2f227`. The inventory below describes **registered screens and source implementations**, not a verified working installation. No native build, GUI workflow, broker account or external data service was tested. Numerical correctness and current provider compatibility remain unverified. Missing advertised capabilities and test requirements are retained in [ROADMAP.md](ROADMAP.md).

| Capability | Code / current availability | Verification status |
|---|---|---|
| Desktop navigation, docking, workspace, settings/themes and local storage | `fincept-qt/src/app/MainWindow.cpp`, `services/workspace/`, `ui/theme/`, `storage/` | Screens/factories and implementations inspected; native GUI/persistence untested |
| Authentication, profile and PIN-related UI | `src/screens/auth/`, `src/auth/`, `src/screens/profile/` under `fincept-qt/` | Client implementation present; authentication service, PIN/security properties and hosted access untested |
| Dashboard, markets, news, watchlist and portfolio | `fincept-qt/src/screens/{dashboard,markets,news,watchlist,portfolio}/`, corresponding services | Registered screens with service/data code; live data, valuation and portfolio calculations untested |
| Equity research, derivatives, M&A, alternative investments and surface analytics | `fincept-qt/src/screens/{equity_research,derivatives,ma_analytics,alt_investments,surface_analytics}/`, `scripts/Analytics/` | Implementations present; DCF/optimization/VaR/Sharpe/pricing correctness not established by tests in this audit |
| Economic/government/DBnomics/AkShare/Asia data and configurable sources | `src/screens/{economics,gov_data,dbnomics,akshare,asia_markets,data_sources,data_mapping}/`, `scripts/*_data.py` | Adapters/screens exist; endpoint access, coverage, licences and “100+ working connectors” count not verified |
| Adanos sentiment overlay | Data-source configuration and equity-research integration | Optional provider-dependent path; no active account or cross-source sentiment request tested |
| Crypto WebSockets, exchange session/account management and order paths | `fincept-qt/src/trading/{websocket,ExchangeService.cpp,ExchangeSessionManager.cpp,AccountManager.cpp}`, `src/screens/crypto_trading/` | Source present, including streaming integrations; live Kraken/Hyperliquid order/data lifecycle untested |
| Equity trading and 16 broker registrations | `fincept-qt/src/trading/BrokerRegistry.cpp`, `brokers/`, `src/screens/equity_trading/` | Registry contains Zerodha, Angel One, Upstox, Fyers, Dhan, Groww, Kotak, IIFL, 5paisa, AliceBlue, Shoonya, Motilal, IBKR, Alpaca, Tradier and Saxo implementations. Registration does not establish complete or working API support; none broker-tested |
| Paper trading and order matching | `fincept-qt/src/trading/PaperTrading.cpp`, `OrderMatcher.cpp`, `UnifiedTrading.cpp` | Local engine code exists; fill/accounting and GUI-to-engine workflows untested |
| Algorithmic trading and historical backtesting | `fincept-qt/src/screens/{algo_trading,backtesting}/`, `src/services/{algo_trading,backtesting}/`, `scripts/Analytics/backtesting/` | Source/provider wrappers present; no strategy/backtest acceptance run |
| AI chat, agent configuration, investment/economic/geopolitical agents and local/provider models | `fincept-qt/src/screens/{chat_mode,agent_config}/`, `src/services/agents/`, `scripts/agents/` | Code/configurations exist; provider availability, active agent counts and response quality untested. Agent-core test collection blocked by missing `agno` in audit environment |
| QuantLib screen and tools | `fincept-qt/src/screens/quantlib/`, `src/services/quantlib/QuantLibClient.cpp` | Client sends `/quantlib/*` requests to `https://api.fincept.in` using app API credentials. Hosted numerical implementation is not in this fork; no API/numerical tests run |
| Visual workflow editor and MCP | `fincept-qt/src/screens/node_editor/`, `src/services/workflow/`, `src/mcp/`, `src/screens/mcp_servers/` | Editor/executor/node handlers and MCP client/tool code exist; graph execution and external tools untested |
| AI Quant Lab ML, factors, RL and HFT research | `fincept-qt/src/screens/ai_quant_lab/`, `src/services/ai_quant_lab/`, `scripts/ai_quant_lab/` | UI dispatch and Python modules exist. `qlib_advanced_models.py` training/prediction generate random synthetic inputs when data is omitted; this is not verified real-market model output or production HFT |
| Global intelligence: maritime, geopolitics, relationship mapping and satellite-data adapters | `fincept-qt/src/screens/{maritime,geopolitics,relationship_map}/`, matching services and `scripts/SATELLITE_GEO_DATA_SOURCES.md` | Screens/adapters present; external AIS/satellite/geopolitical data and analyses untested |
| Reports, notes, file manager, code editor, spreadsheet and trade visualization | Registered factories in `fincept-qt/src/app/MainWindow.cpp`, respective `src/screens/` directories | Implementations present; report/file export, Python editing and spreadsheet workflows untested |
| Forum, support/docs, Alpha Arena and Polymarket | Registered screens and respective services | Client/UI present; hosted/community/prediction services untested |
| Update service and multi-platform installer/build scripts | `fincept-qt/src/services/updater/`, `setup.sh`, `.github/workflows/`, `fincept-qt/packaging/` | Files inspected; no installer, update or platform build validation performed for this fork |

Directories in the table are relative to the indicated `fincept-qt/` root. Related screens being registered does not prove every action works. Exact counts such as “37 AI agents,” “18 modules,” “100+ connectors,” full CFA curriculum coverage and Bloomberg-class performance are unverified upstream claims, not acceptance results.

### Test evidence and gaps

- CMake/Qt6 toolchain is unavailable in this audit environment, so the native application and Qt tests were **not built or run**.
- `fincept-qt/tests/CMakeLists.txt` defines an opt-in Qt Test executable for DataHub. Enable it with `FINCEPT_BUILD_TESTS=ON`; it is not comprehensive screen/broker coverage.
- Python tests exist in `fincept-qt/scripts/agents/finagent_core/tests/` and selected analytics/strategy directories. Attempted resource tests with Python 3.12/pytest were **blocked during collection**: importing `finagent_core` requires the missing `agno` package. This is an environment/dependency block, not a passing or failing behavioral result.
- Build/release workflows exist, but no hosted CI result was retrieved as proof of this snapshot's behavior. Checked-in `test_results_batch1.txt` is historical output, not a fresh full-suite run.
- No real credentials, paid APIs, orders, desktop installation or external writes were used.

After installing the full runtime dependencies, an example Python test command from the repo root is:

```bash
PYTHONPATH=fincept-qt/scripts/agents python3 -m pytest fincept-qt/scripts/agents/finagent_core/tests
```

This command is provided for reproduction; it did not complete in the audit environment. Use the dependency groups in `fincept-qt/resources/requirements*.txt` rather than assuming pytest alone is sufficient.

## Build and installation

The native build declares CMake **3.27+**, C++20, minimum compiler MSVC **19.38**, GCC **12.3**, or Clang **15**. Qt defaults to exact **6.8.3**; optional module handling is in `fincept-qt/CMakeLists.txt`. Python setup checks **3.11+** in `setup.sh`; release tooling also uses Python 3.12. Exact Python 3.11.9, CMake 3.27.7 and Ninja 1.11.1 pins from the former README are not enforced by these source checks. The current macOS preset's deployment target is **13.0**, despite its stale “11.0+” description.

```bash
git clone https://github.com/Kolmen-Strategies/FinceptTerminal.git
cd FinceptTerminal/fincept-qt
# Install the required compiler, CMake/Ninja, Qt kit and Python dependencies first.
cmake --preset linux-release
cmake --build --preset linux-release
./build/linux-release/FinceptTerminal
```

Windows/macOS presets: `win-release`, `macos-release` (and corresponding debug variants). Presets contain expected Qt paths; inspect [fincept-qt/CMakePresets.json](fincept-qt/CMakePresets.json) and use a manual `CMAKE_PREFIX_PATH` if your kit is elsewhere. No local PowerShell profile is supplied that automatically configures Visual Studio; initialize the compiler environment yourself.

For the optional native test target:

```bash
cmake --preset linux-release -DFINCEPT_BUILD_TESTS=ON
cmake --build --preset linux-release --target test_datahub
ctest --test-dir build/linux-release/tests --output-on-failure
```

These build/test commands were source-reviewed, not executed here. `setup.sh` automates Linux/macOS dependency installation/build steps and changes the host; Docker files also exist for an X11-capable Linux environment. Neither was exercised in this audit.

<!-- DOWNLOAD-TABLE-START -->
### Upstream installers

Upstream release linked by this snapshot: **v4.0.2** — [View all releases](https://github.com/Fincept-Corporation/FinceptTerminal/releases/tag/v4.0.2)

| Platform | Download | Run |
|----------|----------|-----|
| **Windows x64** | [FinceptTerminal-Windows-x64-setup.exe](https://github.com/Fincept-Corporation/FinceptTerminal/releases/download/v4.0.2/FinceptTerminal-4.0.2-windows-x64-setup.exe) | Run installer → launch `FinceptTerminal.exe` |
| **Linux x64** | [FinceptTerminal-Linux-x64.run](https://github.com/Fincept-Corporation/FinceptTerminal/releases/download/v4.0.2/FinceptTerminal-4.0.2-linux-x64-setup.run) | `chmod +x` → run installer |
| **macOS Apple Silicon** | [FinceptTerminal-macOS-arm64.dmg](https://github.com/Fincept-Corporation/FinceptTerminal/releases/download/v4.0.2/FinceptTerminal-4.0.2-macos-arm64-setup.dmg) | Open DMG → drag to Applications |
<!-- DOWNLOAD-TABLE-END -->

The installer links above refer to **upstream artifacts**, not builds or tests of this Kolmen fork. Release automation owns that marked block; it does not certify the rest of the feature inventory.

## Scope and roadmap

The previous README mixed current code with dated Q2/Q3 milestones. The current [ROADMAP.md](ROADMAP.md) retains requirements for options-strategy/portfolio completeness, expanded agent coverage, programmatic API, institutional features, mobile/cloud/community features, and validation of integrations and numerical/performance claims. Existing ML training UI is listed above as implemented but untested, rather than marked as a future feature solely because of an old date.

## Contributing and upstream documentation

- [Contributing](docs/CONTRIBUTING.md)
- [C++ contributor guide](docs/CPP_CONTRIBUTOR_GUIDE.md)
- [Python contributor guide](docs/PYTHON_CONTRIBUTOR_GUIDE.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Commercial licensing information](docs/COMMERCIAL_LICENSE.md)

Upstream hosted services, commercial/university offerings, community channels and token programs are external to this source audit. Their advertised availability or terms have not been verified. No token utility/governance/revenue integration is established here.

## License

**Dual Licensed: AGPL-3.0 (Open Source) + Commercial**

### Open Source (AGPL-3.0)
- Free for personal, educational, and non-commercial use
- Requires sharing modifications when distributed or used as network service
- Full source code transparency

### Commercial License
- Required for business use or to access Fincept Data/APIs commercially
- Contact: **support@fincept.in**
- Details: [Commercial License Guide](https://github.com/Fincept-Corporation/FinceptTerminal/blob/main/docs/COMMERCIAL_LICENSE.md)

### Trademarks
"Fincept Terminal" and "Fincept" are trademarks of Fincept Corporation.

© 2025-2026 Fincept Corporation. All rights reserved.
