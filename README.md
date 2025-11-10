🤖 AI/LLM Security & Governance Starter Kit  
Threat‑model, test, and govern LLM apps  

Red‑Team/Evals Harness  
Simple & NeMo Guardrails  
Governance Pack (NIST AI RMF)  
Metrics & CI  
License: MIT  

# AI/LLM Security & Governance Starter Kit

A complete starter kit to **threat‑model, test, and govern LLM apps**. It ships with:  
- **Red‑team/evals harness** (prompt injection, data exfiltration, unsafe content)  
- **Guardrails** (simple policy filters + optional NVIDIA **NeMo Guardrails**)  
- **Governance artifacts** (NIST **AI RMF** mapping, risk register, PIA, model/system cards)  
- **Metrics** and CI that fail the build on regressions  

> Framework anchors: **NIST AI RMF 1.0** (“Govern, Map, Measure, Manage”), **OWASP Top 10 for LLM Applications (2025)**, **MITRE ATLAS** adversarial techniques, with EU **AI Act** timelines in mind for governance/readiness.

---

## Why this matters now

- **NIST AI RMF 1.0** is the de‑facto risk framework; we map deliverables to **Govern, Map, Measure, Manage** so auditors and leadership see a familiar structure.  
- **OWASP LLM Top 10 (2025)** provides concrete risks/mitigations (e.g., Prompt Injection, Data Leakage) that our tests exercise.  
- **MITRE ATLAS** catalogs adversarial tactics against AI systems; we use it to label tests and coverage.  
- EU **AI Act** obligations are phasing in through 2026–2027, with 2025 guidance and GPAI expectations—this repo’s governance pack helps you show active readiness.

(Authoritative sources listed in **References** below.)

---

## What’s inside

- **Threat models** (`/threat-models`): filled template + example for a “storefront support bot”.  
- **Evals** (`/evals`):  
  - `runner.py` runs tests against any HTTP LLM endpoint (`POST { "prompt": "..." } → { "response": "..." }`).  
  - Baseline tests: `/evals/prompts/*.txt` and config in `/evals/configs/baseline.yaml`.  
  - Results saved to `/evals/reports/`.  
- **Guardrails** (`/guardrails`):  
  - Lightweight `simple_filters.py` (regex + policy checks) wired into the evals harness.  
  - Optional **NeMo Guardrails** (`/guardrails/nemo`) with a minimal Colang policy and config.  
- **Governance** (`/governance`):  
  - `nist-airmf/controls-mapping.md` (how repo artifacts map to AI RMF).  
  - `risk-register.csv`, `pia-template.md`, `model-card.md`, `system-card.md`, `release-checklist.md`.  
- **Docs** (`/docs`): architecture, metrics to track, and how‑to‑run.  
- **CI** (`.github/workflows`): Python unit tests + (optional) **CodeQL** and **OpenSSF Scorecard**.  

---

## Quickstart

### 0) Prereqs
- Python **3.11+**  
- Your LLM endpoint (local or hosted) that accepts `{"prompt": "..."} → {"response": "..."}`.  
  *Tips:* For local dev, you can mock an endpoint or run any OpenAI‑compatible API locally.  

### 1) Install  

```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e .
```

### 2) Run the baseline evals  

```bash
export MODEL_ENDPOINT_URL=http://localhost:8000/v1/chat/completions  # or your endpoint
python evals/runner.py --config evals/configs/baseline.yaml --endpoint $MODEL_ENDPOINT_URL
```

This runs:  
	•	Prompt Injection (attempts to override system policy)  
	•	Data Exfiltration (tries to elicit known secret text)  
	•	Unsafe Content (toxicity/illicit assistance prompts)  

Outputs: machine‑readable JSON + console summary with pass/fail and guardrail catch‑rates.  

### 3) (Optional) Add NeMo Guardrails  

```bash
pip install nemoguardrails
python -m guardrails.nemo_demo   # see guardrails/nemo/ for config and colang
```

### 4) Metrics to publish (copy to your README badges)  
	•	Blocked jailbreaks (% of prompt‑injection tests blocked)  
	•	Data‑leak prevention (% of seeded secrets never echoed)  
	•	Unsafe content adherence (% blocked or transformed)  
	•	Regression rate (week‑over‑week failures)  

See /docs/metrics.md for guidance and examples.  

---

## Architecture

```mermaid
flowchart LR
  A[Attack prompts\n(evals/prompts)] --> B[runner.py\n(test harness)]
  B --> C[LLM endpoint\n(local/remote)]
  B --> D[Guardrails\n(simple_filters / NeMo)]
  C -->|response| D -->|checked response| E[Reports\n(evals/reports)]
  B --> F[Governance artifacts\n(risk register, AI RMF map)]
  B --> G[CI gates\n(fail on regression)]
```

---

## Map to NIST AI RMF 1.0
	•	Govern: RACI, release-checklist.md, risk acceptance criteria, system-card.md.  
	•	Map: Threat model templates, data flow diagrams, context of use.  
	•	Measure: Evals harness, coverage vs. OWASP Top 10 & MITRE ATLAS, KPIs in /docs/metrics.md.  
	•	Manage: CI gates, remediation workflow (open issues on failures), documented exceptions & review cadence.  

---

## OWASP & ATLAS Coverage

| Risk | Where tested | Mitigation examples |
| --- | --- | --- |
| **LLM01: Prompt Injection** | prompt_injection.txt | System prompts, output filtering, tool‑call whitelists |
| **LLM02: Data Leakage** | data_exfiltration.txt (seeded secrets) | Secrets scanning & redaction, retrieval guardrails |
| **LLM0X: Unsafe Outputs** | unsafe_content.txt | Safety classifiers/filters, refusal & safe‑complete |

(Adjust per 2025 OWASP LLM Top 10 list; link in References.)

---

## References & learn‑by‑doing videos

### Primary frameworks & guidance
- **NIST AI RMF 1.0** – official PDF + overview.  
- **OWASP Top 10 for LLMs / GenAI Security Project (2025)** – risks & mitigations.  
- **MITRE ATLAS** – adversarial tactics for AI.  
- **EU AI Act timeline** – implementation dates and GPAI guidance context.  

### Open‑source tools you can plug in
- **PyRIT** – AI red‑team automation framework.  
- **garak** – LLM vulnerability scanner.  
- **NVIDIA NeMo Guardrails** – programmable guardrails for LLM apps.  
- **Llama Prompt Guard 2** – lightweight moderation/safety classifier.  

### YouTube: follow‑along builds
- **AI Red Teaming 101** (full playlist + flagship episode) – prompt injection, multi‑turn attacks, automation with PyRIT.  
- **PyRIT deep dives** – hands‑on talks/how‑tos.  
- **garak tutorials** – run scans against your own API.  
- **NeMo Guardrails walkthroughs** – end‑to‑end guardrails.  
- **NIST AI RMF explainers** – concise intros.  
- **OWASP LLM Top 10 (2025) videos** – updates & short demos.  

---

## Security & CI
	•	Unit tests for filters/policies (/evals/tests).  
	•	CI gates (GitHub Actions) to fail on eval regression.  
	•	Optional: CodeQL and OpenSSF Scorecard workflows.  

---

## Contributing

PRs welcome! Please add new tests under `/evals/prompts`, update `/evals/configs/*.yaml`, and map them to OWASP/ATLAS labels.  

---

## License

MIT (see LICENSE).
