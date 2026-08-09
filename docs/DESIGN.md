# Design: Prod-Log Learning

**Project:** `prod-log-learning`  
**Parent system design:** [08 — Fine-Tuning / Eval Data Pipelines](../08-finetuning-eval-data-pipelines.md)

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

> **Watch on YouTube:** [Prod Log Learning — System Design #Shorts](https://youtu.be/nUQEHvg3bC4)
>
> Direct link: **https://youtu.be/nUQEHvg3bC4**

Also available in-repo:
- GIF preview: [`video/design-overview.gif`](./video/design-overview.gif)
- MP4 download: [`video/design-overview.mp4`](./video/design-overview.mp4)
- Narration script: [`video/narration.txt`](./video/narration.txt)

---

**Copyright (c) 2026 Debashis Bhattacharjee. All Rights Reserved.**  
Unauthorized copying or redistribution of this material is prohibited.  
GitHub: [Debashis2007](https://github.com/Debashis2007)

