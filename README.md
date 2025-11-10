# 🤖 AI/LLM Security & Governance Starter Kit  
**Threat‑model, test, and govern LLM apps**

[![Risk Framework: NIST AI RMF](https://img.shields.io/badge/Risk%20Framework-NIST%20AI%20RMF-blue)](https://www.nist.gov/itl/ai-risk-management-framework) [![Red‑team/Evals Harness](https://img.shields.io/badge/Red%E2%80%91team%2FEvals%20Harness-critical?logo=pytest)](#) [![Guardrails Pack](https://img.shields.io/badge/Guardrails-Pack-green)](#) [![Governance Pack: NIST AI RMF](https://img.shields.io/badge/Governance%20Pack-NIST%20AI%20RMF-lightgrey)](#) [![Metrics & CI](https://img.shields.io/badge/Metrics%20%26%20CI-passing-success)](#) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## Overview  
This project provides a complete starter kit to **threat‑model, test, and govern LLM applications**. It ships with:  
- **Red‑team/evals harness** to exercise prompt injection, data exfiltration, unsafe content and other adversarial behaviors.  
- **Guardrails** using simple policy filters plus optional NVIDIA NeMo Guardrails integration.  
- **Governance artifacts** including NIST AI RMF mapping, a risk register template, privacy impact assessments, and model/system cards.  
- **Metrics & CI** that fail the build when regressions are detected.  
  
Framework anchors: NIST AI RMF 1.0 (“Govern, Map, Measure, Manage”), OWASP Top 10 for LLM Applications (2025), MITRE ATLAS adversarial techniques, and the EU AI Act timeline for compliance readiness.

## ⚡️ Quickstart  

```bash
# Clone the repository
git clone https://github.com/xaviermotley/llm-sec-gov.git
cd llm-sec-gov

# Create a virtual environment and install dependencies (if applicable)
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Run baseline evaluations against your LLM endpoint
# (See evals/configs/*.yaml for sample configs)
python evals/run_eval.py --config evals/configs/baseline.yaml
```

## 📚 Architecture  
The kit centers around a few core components:  
- **Threat models** (`./threat-models/`): filled template + example for a “storefront prompt bot”.  
- **Evals** (`./evals`): prompt harness tests against any HTTP LLM endpoint; config driven for risk categories (e.g., “Prompt Injection”, “Data Leakage”).  
- **Guardrails** (`./guardrails`): simple filter-based policies plus optional NVIDIA NeMo Guardrails YAMLs.  
- **Governance** (`./governance`): NIST AI RMF mapping, risk register, model/system cards, privacy impact assessments.  
- **Metrics & CI**: unit tests and GitHub Actions to enforce policy/test thresholds.  

## 📎 Repository Structure  

| Path | Description |
|---|---|
| `threat-models/` | Templates & examples for LLM threat models. |
| `evals/` | Prompt eval harness (configs & tests). |
| `guardrails/` | Policy filters & optional NeMo guardrails configs. |
| `governance/` | NIST AI RMF mapping, risk register, PIA, model/system cards. |
| `metrics/` | Scripts for tracking coverage, detection rate & CI pass/fail. |
| `docs/` | Additional docs & architecture diagrams. |
| `.github/workflows/` | CI definitions for linting, tests, scorecard, CodeQL. |
| `README.md` | This file. |

## 📊 Key Metrics  

- **Coverage**: % of OWASP Top 10 / MITRE ATLAS techniques with at least one eval scenario.  
- **Detection rate**: % of attacks correctly flagged by guardrails/evals.  
- **Governance completeness**: % of required AI RMF artifacts filled out (risk register, PIA, system card).  
- **CI pass rate**: % of commits passing unit tests and security scans.  

## 🛠️ Implementation Steps  

1. **Define threat models**: Fill out `threat-models/ai-threat-model-template.md` with your LLM application context.  
2. **Configure evaluations**: Create YAML configs in `evals/configs/` describing the prompt categories & LLM endpoints to test.  
3. **Run evaluations**: Use the harness (`python evals/run_eval.py`) to exercise your endpoints and observe results.  
4. **Implement guardrails**: Tune `guardrails/filters.py` and optional `nemo_guardrails` YAMLs to enforce policy.  
5. **Fill governance artifacts**: Use templates in `governance/` for your risk register, PIA, and model cards.  
6. **Automate metrics & CI**: Add GitHub Actions to run evals, measure detection rate, and fail on regressions.  

## 🌱 Progressive Add‑Ons  

- **Compliance copilot**: Integrate open-source Vanta MCP server or similar to answer “what controls are failing & why?” (experimental).  
- **Evidence lineage**: Attach raw prompts and responses to eval logs for audit trail.  
- **Org-scale**: Parameterize tests for multiple models/endpoints; incorporate additional frameworks (e.g., ISO 42001).  

## ⚠️ Notes & Gotchas  

- **Auth & rate limits**: When calling external APIs, handle quotas & secrets responsibly (use `.env` + GitHub Secrets).  
- **Don’t over‑promise detection**: Models evolve; continuously update evals/guardrails as new attack techniques emerge.  
- **Share responsibly**: These examples are for educational use; adapt them to your org’s risk appetite and regulatory obligations.  

## 📚 References  

- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)  
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)  
- [MITRE ATLAS Adversarial Threat Landscape for AI](https://atlas.mitre.org/)  
- [EU AI Act Timeline & Draft](https://artificialintelligenceact.eu/)  

## 📄 License  

MIT License © 2025 
