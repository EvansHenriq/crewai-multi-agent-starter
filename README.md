# CrewAI Multi-Agent Starter

> A minimal, well-documented **multi-agent system** built with [CrewAI](https://crewai.com):
> a sequential crew where a **researcher** agent investigates a topic and a
> **reporting analyst** agent turns the findings into a polished Markdown report.
> Scaffolded with **uv** and the **CrewAI CLI** (classic Python/YAML structure) —
> a clean reference for how CrewAI projects are wired.

🇧🇷 _A versão em português está no final._

---

## What it is

A clean starting point for learning and building with CrewAI. Out of the box it
runs a two-agent **pipeline**:

- **Researcher** — gathers the 10 most relevant points about a topic.
- **Reporting Analyst** — expands those points into a full Markdown report (`report.md`).

The agents run in a **sequential process**, so the researcher's output automatically
becomes the analyst's context. Everything that defines *behaviour* (agent personas,
task instructions) lives in **YAML**; everything that defines *wiring* lives in a
small amount of **Python**.

## How it works

```
              inputs: { topic, current_year }
                            │
                            ▼
                      Crew.kickoff()  ──  Process.sequential
                            │
      ┌─────────────────────┴──────────────────────┐
      ▼                                             ▼
  researcher                                 reporting_analyst
  research_task          ──── context ───▶    reporting_task
  "find 10 key bullets"                       "expand into a full report"
      │                                             │
      ▼                                             ▼
  10 bullet points  ───────────────────────▶    report.md
```

## The mental model

CrewAI is built on three abstractions — internalising these makes everything else obvious:

| Concept | What it is | Where it lives |
|---|---|---|
| **Agent** | A worker with a persona (`role` / `goal` / `backstory`) and an LLM | `config/agents.yaml` + `@agent` methods in `crew.py` |
| **Task** | A unit of work: an instruction (`description`) + a definition of done (`expected_output`) | `config/tasks.yaml` + `@task` methods in `crew.py` |
| **Crew** | The orchestrator that runs agents over tasks with a `Process` | the `@crew` method in `crew.py` |

The split between **declarative config (YAML)** and **wiring (Python)** is deliberate:
you can iterate on a persona or a task's "definition of done" without touching code.

## Project structure

```
src/learn_crewai/
├── main.py            # entry points (run / train / replay / test) + inputs
├── crew.py            # @CrewBase class: wires agents + tasks into a Crew
├── config/
│   ├── agents.yaml    # agent personas (role / goal / backstory)
│   └── tasks.yaml     # task instructions + expected_output
└── tools/
    └── custom_tool.py # example BaseTool (Pydantic args schema)
knowledge/             # example knowledge source
pyproject.toml         # dependencies + CLI entry points (run_crew → main:run)
```

## Quickstart

Prerequisites: [uv](https://docs.astral.sh/uv/), Python 3.12, and an OpenAI API key.

```bash
# 1. Install dependencies (creates .venv + uv.lock)
crewai install

# 2. Configure your key — create a .env file with:
#      MODEL=gpt-4o-mini
#      OPENAI_API_KEY=sk-...

# 3. Run the crew (writes report.md in the project root)
crewai run
```

Change the `topic` in [`main.py`](src/learn_crewai/main.py) to research anything you like.

## Swap the LLM provider in one line

CrewAI routes through [LiteLLM](https://docs.litellm.ai), so switching providers is
just the `MODEL` variable in `.env` — the agent and task code never changes:

```dotenv
MODEL=gpt-4o-mini          # OpenAI (default)
# MODEL=ollama/llama3      # local Ollama — no API key, runs offline
```

Any LiteLLM-supported provider works (OpenAI, Anthropic, Mistral, Ollama, and many
more) — set `MODEL` and supply that provider's API key. Some native providers need
an extra (e.g. an additional install) and their own key.

## Tech stack

Python 3.12 · [CrewAI](https://crewai.com) · LiteLLM · Pydantic · **uv** · YAML-driven config

## License

[MIT](LICENSE).

---

# 🇧🇷 Português

> Um **sistema multi-agente** mínimo e bem documentado, feito com
> [CrewAI](https://crewai.com): uma crew sequencial onde um agente **pesquisador**
> investiga um tópico e um **analista de relatórios** transforma os achados num
> relatório Markdown. Criado com **uv** e a **CLI do CrewAI** (estrutura clássica
> Python/YAML).

## O que é

Um ponto de partida limpo para aprender e construir com CrewAI. De fábrica, roda um
pipeline de dois agentes:

- **Pesquisador** — levanta os 10 pontos mais relevantes sobre um tópico.
- **Analista de Relatórios** — expande esses pontos num relatório Markdown completo (`report.md`).

Os agentes rodam em **processo sequencial**: a saída do pesquisador vira
automaticamente o contexto do analista. O *comportamento* (personas, instruções)
fica em **YAML**; a *fiação*, em pouco **Python**.

## Modelo mental

| Conceito | O que é | Onde fica |
|---|---|---|
| **Agent** | Um trabalhador com persona (`role`/`goal`/`backstory`) e um LLM | `config/agents.yaml` + métodos `@agent` |
| **Task** | Uma unidade de trabalho: instrução (`description`) + critério de pronto (`expected_output`) | `config/tasks.yaml` + métodos `@task` |
| **Crew** | O orquestrador que roda agents sobre tasks com um `Process` | método `@crew` em `crew.py` |

A separação entre **config declarativa (YAML)** e **fiação (Python)** é proposital:
dá para iterar numa persona ou no "critério de pronto" de uma task sem tocar no código.

## Como rodar

Pré-requisitos: [uv](https://docs.astral.sh/uv/), Python 3.12 e uma chave da OpenAI.

```bash
crewai install                # instala deps (.venv + uv.lock)
# crie um .env com MODEL=gpt-4o-mini e OPENAI_API_KEY=sk-...
crewai run                    # gera report.md na raiz do projeto
```

Troque o `topic` em [`main.py`](src/learn_crewai/main.py) para pesquisar qualquer
assunto. Para trocar de provedor de LLM, basta mudar a linha `MODEL=` no `.env`.

## Licença

[MIT](LICENSE).
