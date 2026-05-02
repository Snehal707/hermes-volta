# Agent Context

This is the public, cleaned version of the local agent context used while developing Hermes Volta.

## Project

Hermes Volta is a circuit design agent built on Hermes Agent. It converts plain-English analog circuit requests into component values, PySpice/Ngspice simulations, KiCad/SKiDL artifacts, Bode plots, waveform plots, JLCPCB-oriented BOM hints, Telegram delivery, dashboard views, and reusable verified design recipes.

## Runtime Boundary

Hermes Agent is the orchestration/runtime layer. Hermes Volta is the project-specific domain layer.

The local development checkout may contain:

```text
hermes-agent/
```

That directory is intentionally not committed. It is an external runtime checkout and virtual environment, not source code owned by this repo.

## Key Integration Points

| Path | Role |
| --- | --- |
| `skills/volta/SKILL.md` | Hermes skill definition and workflow. |
| `skills/volta/references/` | Filter math, KiCad footprint guidance, component recipes, and extended operating docs. |
| `sim/faraday_pipeline.py` | Main pipeline entry point for full design execution. |
| `sim/simulate.py` | PySpice/Ngspice simulation engine. |
| `sim/netlist.py` | KiCad netlist generator. |
| `sim/pcb_export.py` | KiCad CLI export wrapper. |
| `sim/report.py` | Cutoff report writer. |
| `dashboard/api.py` | FastAPI dashboard/API layer. |
| `tests/smoke_test.py` | End-to-end smoke test suite. |

## Environment Notes

- Target OS: WSL2/Linux-style shell for demos.
- Circuit simulation: PySpice + Ngspice.
- EDA/export: KiCad CLI when available.
- Generated artifacts go under `outputs/`.
- `outputs/`, local memory, logs, virtual environments, and `hermes-agent/` are ignored by git.

## Public Repo Discipline

Keep the public repository focused on:

- Source code
- Skill files
- Tests
- Documentation
- Small curated demo artifacts

Do not commit:

- Bot tokens
- Chat IDs
- `.env` files
- Full generated `outputs/` dumps
- Local virtual environments
- External Hermes Agent checkouts
