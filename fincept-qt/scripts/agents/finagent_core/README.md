# FinAgent Core

Configurable Agno-based agent runtime used by Fincept's Python agent dispatch code. This README describes source implementations, not a verified hosted service or a tested provider/model catalog.

## Features and evidence — 2026-11-25

| Capability | Source | Availability / verification |
|---|---|---|
| Single-agent execution and builder | `core_agent.py` (`CoreAgent.run`, `CoreAgentBuilder`) | Implemented facade; no end-to-end run tested |
| Teams and workflows | `CoreAgent.run_team`, `run_workflow`, `modules/` | Code routes members through persona registry; unit tests exist in `tests/test_team_workflow_registry.py`, not completed in this audit |
| Per-user/persona runtime cache and resource paths | `persona_registry.py`, `persona_runtime.py`, `resources.py` | Source/tests exist; attempted resource test blocked during collection by missing `agno` |
| Factory, config and agent discovery | `agent_factory.py`, `config_loader.py`, `agent_loader.py`, `configs/` | Present; not a verified count of loadable personas or supported provider models |
| Model, tool, embedder and vector-store configuration | `registries/`, `tools/`, `modules/` | Provider/optional-package-dependent code; all combinations untested |
| Memory, sessions, knowledge and reasoning | `modules/`, `repositories.py` | Implementations present; external database/vector-store behavior and persistence acceptance untested |
| Intent routing and execution plans | `super_agent.py`, `execution_planner.py`, `task_state.py` | Implemented components; full chained operation untested |
| Paper trading | `paper_trading_bridge.py` | Local bridge implementation; no complete fill/accounting test run here |
| Application dispatch/streaming | `main.py`, `core_agent_stream.py` | JSON dispatch/stream handling source; no desktop integration verified |

The facade uses persona runtimes and a registry; the former description of a single agent recreated on every config change was incomplete. The former `python agent_manager.py ...` example referenced a file absent from this directory. Follow `main.py`'s actual action/payload dispatch contract rather than that legacy invocation. No API keys should be embedded in examples or passed into committed configuration files.

## Tests and dependencies

Tests are in `tests/`. The audit attempted the resource tests with Python 3.12/pytest; importing this package reached `agent_factory.py` and failed to import `agno`. No passing test result is claimed. Install the intended Agno/provider/tool dependencies from `fincept-qt/resources/requirements*.txt` first; optional tools and models require their respective packages and credentials.

From repository root, after provisioning dependencies:

```bash
PYTHONPATH=fincept-qt/scripts/agents python3 -m pytest fincept-qt/scripts/agents/finagent_core/tests
```

This command was not completed during the audit. [Repository feature inventory](../../../../README.md#feature-verification--2026-11-25) and [roadmap](../../../../ROADMAP.md) retain required agent compatibility, numerical/output, provider and integration verification. Source definitions do not establish investment performance, current model availability or enforcement of all intended isolation guarantees.
