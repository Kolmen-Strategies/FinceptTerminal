# Agent configurations and runtime

Agent-related sources for Fincept Terminal. This tree contains a configurable Agno-based FinAgent Core, persona/model/tool registries, category definitions and additional hedge-fund/research implementations. It is not a verified list of active deployed agents.

## Current inventory — 2026-11-25

| Component | Actual source | Status |
|---|---|---|
| Configurable agent facade | `finagent_core/core_agent.py` (`CoreAgent`, `CoreAgentBuilder`) | Source methods for single agent, team, workflow and analysis tasks; not runtime-tested in this audit |
| Persona isolation and caching | `finagent_core/persona_runtime.py`, `persona_registry.py`, `resources.py` | Per-user/persona paths, runtime lifecycle and caching code; tests exist, collection blocked by missing `agno` |
| Agent discovery/configuration | `finagent_core/agent_loader.py`, `config_loader.py`, `configs/` | Config-driven definitions and loader code; loadable count/provider compatibility not verified |
| Model/tool/vector/embedder support | `finagent_core/registries/`, `tools/`, `agent_factory.py` | Optional dependency/provider-backed implementations; counts are not availability guarantees |
| Routing/plans and persistence | `finagent_core/super_agent.py`, `execution_planner.py`, `repositories.py` | Implemented sources; no complete run tested |
| Paper-trading bridge | `finagent_core/paper_trading_bridge.py` | Local bridge code; no real or simulated round trip verified |
| C++/Python dispatch | `finagent_core/main.py`, `core_agent_stream.py` | Dispatch/streaming entry points; not proof of a working desktop-to-agent flow |
| Investment, economic, geopolitical and hedge-fund definitions | `TraderInvestorsAgent/`, `EconomicAgents/`, `GeopoliticsAgents/`, `hedgeFundAgents/` | Some categories use `configs/agent_definitions.json`; do not assume every persona has a matching Python class/module |
| Additional research frameworks | `deepagents/`, `rdagents/` | Source present; integration/dependency coverage not established |

The previous README imported missing class-style modules and `agents.agent_manager.AgentManager`. Those examples do not match this tree. It also listed nonexistent core paths such as `base_agent.py` and `database/db_manager.py`. Use the actual sources above; legacy compatibility, agent-count verification and workflow/debate acceptance remain [roadmap requirements](../../../ROADMAP.md).

## Verification

The root [feature audit](../../../README.md#feature-verification--2026-11-25) records full repository scope and limitations. Tests are under `finagent_core/tests/`; attempting `test_resources.py` with Python 3.12/pytest stopped during import because `agno` was unavailable. No agent output, investment decision, provider request or local Ollama model was verified. Install the optional runtime packages in the repository's requirements groups before attempting these tests.

[FinAgent Core inventory](finagent_core/README.md) describes current entry points. Model IDs/configuration examples elsewhere in this tree are upstream snapshots, not current provider support certificates.
