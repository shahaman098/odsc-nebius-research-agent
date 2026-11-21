# CiteTrail — Research Agent

> ODSC × Nebius Hackathon submission · Challenge: *Build a research agent that brings its sources*

**One-line pitch:** CiteTrail answers a research question with live web evidence and inline citations, for developers who need verifiable answers fast.

- **Demo video (2–3 min):** see `docs/DEMO.md` / release asset after push
- **Team:** Aman Kumar Sah (`shahaman098`) · solo

## The problem
Developers and students often need a quick, sourced answer (API docs, product facts, current launches). Generic chatbots invent links. CiteTrail forces every claim to sit on a live Tavily result and a Nebius Token Factory synthesis pass.

## How it works
```
question → plan 1–3 queries (Nebius) → Tavily search → dedupe URLs → Nebius synthesis with [n] citations
```
- **Query planning:** Nebius proposes concrete search queries from the user question.
- **Evidence handling:** URLs are deduped; source text is labeled and passed as data only.
- **Safety:** Source text is treated as untrusted data, not instructions.
- **Trade-offs:** Caps results (~6) for latency/cost; if search fails or is empty, it refuses to invent citations.

## Required services
| Requirement | How we use it |
|---|---|
| Nebius Token Factory (inference) | Model: `Qwen/Qwen3-30B-A3B-Instruct-2507` — query planning + answer synthesis |
| Tavily (real-time web search) | Live web retrieval; source URLs survive into the final Sources list |

## Setup
```bash
git clone https://github.com/shahaman098/odsc-nebius-research-agent
cd odsc-nebius-research-agent
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # add NEBIUS_API_KEY and TAVILY_API_KEY — never commit them
python src/agent.py "Your research question here"
```

## Example
**Question:** What is Nebius Token Factory and how do developers call it with an OpenAI-compatible API?

**Answer (excerpt):** Nebius Token Factory provides an OpenAI-compatible API for inference… Developers authenticate with a Bearer API key… [see `runs/demo-success.txt`]

**Sources:** live URLs from Tavily (docs.tokenfactory.nebius.com, etc.)

## Failure / insufficient-evidence case
When search returns nothing useful, CiteTrail replies with an explicit insufficient-evidence message and does not invent citations. See `runs/demo-failure.txt`.

## Judging criteria map
| Criterion | Weight | Where to look |
|---|---|---|
| Sourced and verifiable answers | 30% | Example + `runs/demo-success.txt` |
| Works end to end | 25% | Setup, live run |
| Usefulness | 20% | The problem |
| Agent design | 15% | How it works + `src/agent.py` |
| Use of Token Factory and Tavily | 10% | Required services |

## License
MIT
