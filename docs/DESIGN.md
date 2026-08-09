# Design: Prod-Log Learning

**Project:** `prod-log-learning`  
**Parent system design:** `08-finetuning-eval-data-pipelines.md`

## 1. What this POC demonstrates

Policy-permitted prod sampling with ZDR skip, PII redact, quarantine then bless.

## 2. Architecture (POC)

```text
POST /sample → ZDR skip | redact → quarantine
POST /bless → mark blessed
```

## 3. Patterns used (and why)

| Pattern | Why used | Where in code |
|---------|----------|---------------|
| ZDR exclusion | Enterprise zero-retention must be absolute. | `zdr=true` skip. |
| Redaction before land | Reduce PII in training lakes. | Email regex redact. |
| Quarantine then bless | Poison resistance workflow. | `status` transitions. |

## 4. Key endpoints

`GET /health`, `POST /sample`, `POST /bless`

## 5. Tradeoffs / POC limits

Redaction is regex-demo quality only.

## 6. How to run

See the **Run (self-contained POC)** section in [`../README.md`](../README.md).

This folder is self-contained and can be published as its own GitHub repository.

## 7. Design walkthrough video

Narrated with **ElevenLabs Debpro voice** and Debpro still image (via [GitaProject](/Users/deb/Development/GenAI/GitaProject)):

- Video: [`video/design-overview.mp4`](./video/design-overview.mp4)
- Script: [`video/narration.txt`](./video/narration.txt)

