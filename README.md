# Use Case: Prod-Log Learning

**YouTube walkthrough:** [Prod Log Learning — System Design #Shorts](https://youtu.be/nUQEHvg3bC4)

**Design doc:** [docs/DESIGN.md](./docs/DESIGN.md) — architecture, patterns, and why.


**Parent system design:** [08 — Fine-Tuning / Eval Data Pipelines](../08-finetuning-eval-data-pipelines.md)

## Users & problem

Platform ML wants to learn from production interactions where policy allows. Privacy, consent, and poison resistance dominate.

## Requirements & SLOs

| Requirement | Target |
|-------------|--------|
| Consent/policy | Explicit legal basis |
| PII | Redact/anonymize before land |
| Poison | Quarantine + anomaly filters |
| Opt-out | Honor quickly |

## Design (from parent)

```
Prod sample (policy-permitting) → redact
  → landing immutable raw (restricted)
  → validate/quarantine → blessed subset
  → train only via registry
```

## Specializations

| Concern | Prod-log choice |
|---------|-----------------|
| Access | Break-glass on raw |
| Bias | Stratify; don’t overfit power users |
| Feedback | Thumbs signals as weak labels |
| Enterprise ZDR | Exclude those tenants entirely |

## Failure modes

- ZDR tenant sampled → tenant policy flag at emit time.
- Adversarial users poisoning → rate limits + anomaly quarantine.
- Undocumented use → purpose binding in manifests.




## Design walkthrough (opens on GitHub)

> **Watch on YouTube:** [Prod Log Learning — System Design #Shorts](https://youtu.be/nUQEHvg3bC4)


![Design overview](docs/video/design-overview.gif)

Full narrated video (download): [docs/video/design-overview.mp4](docs/video/design-overview.mp4)

## Run (self-contained POC)

This folder is a **standalone** project (safe to split into its own GitHub repo).

```bash
cd prod-log-learning
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
PYTHONPATH=. python -m uvicorn app.main:app --reload --port 8000
```

```bash
curl -s http://127.0.0.1:8000/health | jq
```

curl -s -X POST http://127.0.0.1:8000/sample -H 'Content-Type: application/json' -d '{"tenant":"consumer","text":"my email is a@b.com","zdr":false}' | jq

---

**Copyright (c) 2026 Debashis Bhattacharjee. All Rights Reserved.**  
Unauthorized copying or redistribution of this material is prohibited.  
GitHub: [Debashis2007](https://github.com/Debashis2007)

