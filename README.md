# Mycroft-n8n-Production-Grade-Collaboration

## n8n Plans & Collaboration Guide

## Overview

n8n is a flexible workflow automation tool that supports both **n8n Cloud** (hosted by n8n) and **Self-Hosted** deployments. Plans vary based on collaboration features, execution limits, auditability, and enterprise-grade needs.

---

##  Plan Summary Table

| Plan Type                 | Pricing (Cloud)       | Collaboration Features                                         | Best Use Cases                                            |
|---------------------------|------------------------|----------------------------------------------------------------|-----------------------------------------------------------|
| **Community Edition**     | Free (self-hosted)     | Basic use—no projects, no credential sharing, single user.  | Solo devs or one-off automations without team needs.      |
| **Starter (Cloud)**       | US $24/mo (annually US $20) for 2.5K executions/month. | 1 shared project, unlimited users, 5 concurrent runs, forum support.  | Teams trialing MIDI-scale automations with DevOps buffer. |
| **Pro (Cloud)**           | Contact Sales          | 3 shared projects, 20 concurrent runs, 7-day history, admin roles, search, global variables. | Small teams scaling features and collaboration.           |
| **Business (Self-Hosted)**| Contact Sales          | Git version control, environments (dev/staging/prod), SSO, LDAP, 6 projects, insights 30 days. | Growing orgs needing structured workflows and security.   |
| **Enterprise**            | Contact Sales (cloud or self-hosted) | Unlimited projects, 200+ concurrent runs, 365-day history, external secrets, log streaming, API key scoping, SLA support. | Regulated orgs, financial tech, high-compliance teams.    |

---

##  Pricing Philosophy

- All **Cloud plans** now include **unlimited workflows, steps, and users**. You pay **only for executions**, not complexity. :contentReference[oaicite:7]{index=7}  
- **Enterprise pricing** is execution-based (not workflow-based), offering budget predictability at scale. :contentReference[oaicite:8]{index=8}

---

##  Collaboration Features by Plan

| Plan                  | Projects & Sharing         | Git & Environments           | SSO / LDAP              | Credential Sharing             | Usage Insights / Metrics         |
|-----------------------|----------------------------|------------------------------|--------------------------|-------------------------------|----------------------------------|
| Community Edition     | No                          | No                           | No                       | No                            | Limited (24-hour history) :contentReference[oaicite:9]{index=9} |
| Starter               | 1 project                   | No                           | No                       | Limited                       | 1-day history                    |
| Pro                   | 3 projects                  | No                           | No                       | Yes                           | 7-day history, execution search |
| Business (Self-Host)  | 6 projects                  | Yes (dev/staging/prod)       | Yes (SSO/LDAP)           | Yes                           | 30-day insights                 |
| Enterprise            | Unlimited                   | Yes                          | Yes, with control        | Yes, secure & scoped          | 365-day retention, log streaming |

---

##  Choosing the Right Plan

- **Community Edition**: Great for testing or single-user projects; lacks collaboration tooling.  
- **Starter (Cloud)**: Best for small teams trying out shared automation without full CI/CD.  
- **Pro (Cloud)**: Ideal for expanding teams needing audit history and simple role control.  
- **Business (Self-Hosted)**: Perfect for structured DevOps pipelines, secure credentials, and multi-user environment separation.  
- **Enterprise**: Suited for mission-critical systems, compliance-heavy industries (e.g., finance, healthcare), and teams requiring advanced security and support.

---

##  Best Practices by Plan

1. **Community**: Use git to version JSON exports manually; keep credentials encrypted or out-of-band.  
2. **Starter / Pro**: Use shared project(s); editors can drag/drop/access without exposing secrets; rely on forum for support.  
3. **Business**: Implement Git-backed environments, credential references (e.g., Vault), role-based access, team RBAC, and full smoke-test pipelines.  
4. **Enterprise**: Tiered environments, detailed logging to SIEM, API key scoping, credential vaulting, audit trails, and SLA assurances.

---

##  Example Scenarios & Plan Fit

- **Use Case: Mycroft agent pipelines**  
  - Development & experimentation → **Starter** or **Community**  
  - Collaboration with separate teams & environments → **Business**  
  - Full production-grade risk/compliance → **Enterprise**

- **Use Case: AI job scheduling with secrets**  
  - For solo researchers → **Community** is fine  
  - Shared pipelines/data access → move to **Pro** for credential sharing  
  - Production-grade monitoring → **Business** with insights & Gitflow

---

##  Planning

n8n offers an execution-centric, scalable pricing model that encourages experimentation while scaling collaboration via structured plans. Choose the plan that matches your maturity level:

- Start small—**Community / Starter**
- Grow collaboration safely—**Pro**
- Build with structure—**Business**
- Go enterprise-grade—**Enterprise**

---


# Mycroft × n8n — Production‑Grade Collaboration README

> Using AI to invest in AI — safely, collaboratively, and at scale.

This repository standardizes **how we collaborate in n8n** for the Mycroft project (and any multi‑team automation initiative). It covers environments, Git flow, RBAC, secrets, CI/CD, testing, monitoring, and integrations used by our four agent categories: **Intelligence, Analytical, Portfolio, Advisory**.

---

## Table of Contents

1. [Goals & Non‑Goals](#goals--non-goals)
2. [Architecture & Repo Layout](#architecture--repo-layout)
3. [Environments & Branching](#environments--branching)
4. [Roles, Projects, and Access](#roles-projects-and-access)
5. [Secrets, Credentials, and Environments](#secrets-credentials-and-environments)
6. [Local Dev Setup](#local-dev-setup)
7. [Workflow Design Standards](#workflow-design-standards)
8. [CI/CD (GitHub Actions)](#cicd-github-actions)
9. [Promotion & Release Process](#promotion--release-process)
10. [Testing Strategy](#testing-strategy)
11. [Monitoring, Logging & Alerting](#monitoring-logging--alerting)
12. [Security & Compliance](#security--compliance)
13. [Plan Selection: Cloud vs Self‑Hosted](#plan-selection-cloud-vs-self-hosted)
14. [Integrations & Connections](#integrations--connections)
15. [Collaboration Routines](#collaboration-routines)
16. [Common Patterns & Reusable Building Blocks](#common-patterns--reusable-building-blocks)
17. [Performance & Reliability](#performance--reliability)
18. [Troubleshooting & FAQs](#troubleshooting--faqs)
19. [Appendix: Templates & Snippets](#appendix-templates--snippets)

---

## Goals & Non‑Goals

### Goals

* Reproducible **multi‑env** development: dev → staging → prod.
* **Safe collaboration**: clear roles, shared credentials without secret exposure, peer reviews.
* **Automated delivery**: validate → integrate → promote with minimal manual steps.
* **Operational excellence**: observability, auditing, disaster recovery.

### Non‑Goals

* Replace financial/compliance policies; instead, integrate with them.
* Offer pricing commitments; see vendor site for latest details.

---

## Architecture & Repo Layout

```
mycroft-n8n/
├─ workflows/
│  ├─ intelligence/
│  │  ├─ edgar_10q_monitor.json
│  │  └─ patent_watch.json
│  ├─ analytical/
│  │  └─ rag_company_profile.json
│  ├─ portfolio/
│  │  └─ factor_backtest_weekly.json
│  ├─ advisory/
│  │  └─ goal_explainer_chat.json
│  └─ commons/   # subworkflows, error router, slack notifier, schema validators
├─ env/
│  ├─ dev.env.json
│  ├─ staging.env.json
│  └─ prod.env.json
├─ docs/
│  ├─ CHANGELOG.md
│  └─ RUNBOOKS.md
├─ .github/
│  └─ workflows/
│     ├─ validate-n8n.yml
│     ├─ promote-staging.yml
│     └─ promote-prod.yml
└─ tools/
   ├─ jsonlint.mjs
   └─ schema/
      └─ workflow.schema.json
```

**Naming convention**: `{domain}_{verb}_{cadence}.json` (e.g., `factor_backtest_weekly.json`).

---

## Environments & Branching

We map **Git branches to environments**:

* `dev`: day‑to‑day edits; permissive credentials with sandbox data.
* `staging`: integration test with staging DBs/tokens; auto smoke tests.
* `prod`: production; promotion only via PRs and approvals.

**Promotion path**: `feature/*` → PR to `dev` → PR to `staging` → PR to `prod`.

**Sync model**: Each n8n instance (Dev/Staging/Prod) tracks its corresponding Git branch. Changes are pulled and applied by the instance. Project Admins can push; instance owners/admins manage the connection.

---

## Roles, Projects, and Access

We organize work by **Projects** that mirror agent categories plus a **Commons** project for shared utilities.

* `Intelligence` (news/filings/social ingestors)
* `Analytical` (feature extraction, embeddings, RAG)
* `Portfolio` (backtests, scoring, rebalancing proposals)
* `Advisory` (conversational flows, education)
* `Commons` (error router, notifications, reusable transforms)

**Role matrix (per project)**

| Role   | Can Edit Workflows | Use Shared Creds | See Secret Values | Approve Releases |
| ------ | ------------------ | ---------------- | ----------------- | ---------------- |
| Admin  | ✅                  | ✅                | ❌ (masked)        | ✅                |
| Editor | ✅                  | ✅                | ❌                 | ❌                |
| Viewer | ❌                  | (N/A)            | ❌                 | ❌                |

> Assign the **least privilege** needed. Default new members to **Viewer**, elevate per project.

**Ownership rule**: Workflow creator remains owner; plan ahead for off‑boarding (delete user → transfer ownership) via runbooks.

---

## Secrets, Credentials, and Environments

**Golden rules**

* Never commit raw secrets. Store them in a secret manager (Vault, AWS Secrets Manager, GCP Secret Manager) and reference by **IDs** or environment variables.
* Use **credential sharing** so teammates can *use* connectors without reading the secret.
* Keep separate credentials per environment (GitHub‑Dev vs GitHub‑Prod, etc.).

**Credential IDs** (examples)

* `VAULT:openai/api_key`
* `VAULT:slack/bot_token`
* `VAULT:postgres/prod_rw`
* `VAULT:hf/token`

**Environment overlays**

* `env/dev.env.json`, `env/staging.env.json`, `env/prod.env.json` define non‑secret toggles (feature flags, limits, dataset selectors).
* Sensitive values are injected at runtime via secret manager or `_FILE` env patterns.

---

## Local Dev Setup

1. **Prereqs**: Docker, Git, Node 18+ (for tooling), access to Dev n8n instance.
2. **Clone** the repo and create a feature branch.
3. **Run n8n (optional local)** with a sample compose:

```yaml
# docker-compose.n8n.yml (local only)
version: '3.8'
services:
  n8n:
    image: n8nio/n8n:latest
    ports: ["5678:5678"]
    environment:
      - N8N_ENCRYPTION_KEY=${N8N_ENCRYPTION_KEY}
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=db
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=n8n
    depends_on: [db]
  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=n8n
      - POSTGRES_DB=n8n
    volumes:
      - pgdata:/var/lib/postgresql/data
volumes:
  pgdata:
```

4. **Connect to Dev** instance (recommended) and develop against shared test data.

---

## Workflow Design Standards

**Triggers**

* Prefer **Webhook** (for API‑style invocations) or **Cron/Schedule** for periodic jobs.
* Upstream events (GitHub, Slack) hit Webhook endpoints; respond fast and offload heavy work to a **Queue Subworkflow**.

**Error handling**

* All workflows end in the **Error Router** subworkflow that logs context and notifies Slack/Email with correlation IDs.
* Use **Try/Catch** with fallback paths; protect external calls with retries + backoff.

**Data contracts**

* Intelligence writes normalized rows to `staging.company_events` (schema in `/tools/schema/`).
* Analytical consumes `staging` and emits curated `warehouse.*` tables; enforce schemas with a **Validator** subworkflow.

**Idempotency**

* Use deterministic keys (e.g., `cik+filing_date+form_type`) to avoid duplicates.

**Naming**

* Nodes: `verb_object` (e.g., `fetch_edgar`, `parse_html`, `notify_slack`).
* Credentials: `{service}_{env}` (e.g., `slack_bot_dev`).

---

## CI/CD (GitHub Actions)

### 1) Validate on PR

* Lint JSON exports; forbid raw secrets; check node compatibility.

```yaml
# .github/workflows/validate-n8n.yml
name: Validate n8n
on: [pull_request]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20 }
      - run: npm ci
      - run: node tools/jsonlint.mjs workflows
      - name: Secret scan
        uses: zricethezav/gitleaks-action@v2
      - name: Schema check
        run: node tools/validate-schema.mjs workflows tools/schema/workflow.schema.json
```

### 2) Promote to Staging and run smoke tests

```yaml
# .github/workflows/promote-staging.yml
name: Promote to Staging
on:
  push:
    branches: [ "staging" ]
jobs:
  smoke:
    runs-on: ubuntu-latest
    steps:
      - name: Hit staging smoke webhook (EDGAR monitor)
        run: |
          curl -sS -X POST "$STAGING_EDGAR_SMOKE_URL" \
            -H 'Content-Type: application/json' \
            -d '{"cik":"0000320193","form":"10-Q","limit":1}'
      - name: Audit staging instance
        run: |
          curl -sS -X POST "$STAGING_N8N_API/audit" \
            -H "Authorization: Bearer $N8N_API_TOKEN"
```

### 3) Promote to Prod after approvals

```yaml
# .github/workflows/promote-prod.yml
name: Promote to Prod
on:
  push:
    branches: [ "prod" ]
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - name: Warm up prod endpoints
        run: |
          curl -sS "$PROD_HEALTHCHECK_URL" || exit 1
      - name: Trigger post-deploy check
        run: |
          curl -sS -X POST "$PROD_POST_DEPLOY_CHECK_URL"
```

> Note: We **invoke workflows via Webhook triggers** for tests. Avoid internal/undocumented execution endpoints; keep execution semantics explicit.

---

## Promotion & Release Process

1. Feature branch merged → **Dev** auto‑syncs.
2. PR to `staging` → smoke tests ping staging webhooks; failures block release.
3. PR to `prod` → post‑deploy checks, alert to #mycroft‑ops.
4. Tag release in `docs/CHANGELOG.md` with impacted workflows and config deltas.

---

## Testing Strategy

* **Unit‑like tests** in Code nodes (pure transforms) using sample payloads.
* **Contract tests**: validate inputs/outputs against JSON Schemas.
* **Smoke tests**: minimal end‑to‑end runs via staging webhooks.
* **Canary runs**: route 1/N events to staging for high‑risk changes.

---

## Monitoring, Logging & Alerting

* Centralize logs (stdout) into your SIEM (ELK/Datadog/CloudWatch). Include workflow name, run ID, project, env.
* Error Router posts to Slack channel with a deep link to the execution.
* Weekly **Security Audit** job posts findings to #mycroft‑ops.

---

## Security & Compliance

* **Least privilege** projects/roles; default **Viewer**.
* **SSO (SAML)** on enterprise instances; enforce 2FA where available.
* Rotate credentials quarterly; secrets live only in secret manager.
* Monthly **audit** and quarterly **DR drill** (restore from backup).

---

## Plan Selection: Cloud vs Self‑Hosted

| Scenario                                      | Recommended Plan            | Rationale                                                                          |
| --------------------------------------------- | --------------------------- | ---------------------------------------------------------------------------------- |
| Solo dev or tiny internal prototype           | **Self‑hosted Community**   | Full control, no cost; lacks RBAC/projects.                                        |
| Small team, minimal ops                       | **n8n Cloud (Starter/Pro)** | Managed hosting, credential sharing, public API access (paid), predictable ops.    |
| Multi‑team, regulated data, SSO, environments | **Self‑hosted Enterprise**  | RBAC projects/roles, SAML SSO, Git‑backed environments, advanced logging & audits. |

> Pricing and plan names evolve; check vendor site for latest execution‑based tiers and limits.

---

## Integrations & Connections

**Data & Finance**

* SEC EDGAR (HTTP Request + Code)
* Market data APIs (Yahoo Finance, Alpha Vantage)

**Storage & Compute**

* Postgres (OLTP), Warehouse of choice (BigQuery/Redshift/Snowflake)
* Vector DB (Pinecone/Weaviate/pgvector)

**LLMs & AI**

* OpenAI / Azure OpenAI / Anthropic / Hugging Face Inference
* Prompt templates versioned under `workflows/advisory/prompts/`

**Collab & Alerts**

* Slack, Email (SMTP), GitHub webhooks

**Credential map example**

```
slack_bot_dev → VAULT:slack/bot_token
postgres_rw_staging → VAULT:postgres/staging_rw
openai_dev → VAULT:openai/api_key
hf_token → VAULT:hf/token
```

---

## Collaboration Routines

* **PR template** includes workflow screenshots, test evidence, rollback plan.
* **Changelog discipline**: human‑readable summary per change.
* **RFCs** for schema changes; 2 reviewers minimum for Portfolio.
* **Office hours** weekly for cross‑agent alignment.

---

## Common Patterns & Reusable Building Blocks

**Error Router (subworkflow)**

* Inputs: `err`, `context`, `run_id`, `project`, `env`
* Actions: log → Slack notify → ticket (optional) → store metrics

**Queue Offload**

* Webhook → quick 200 → Enqueue payload → Worker subworkflow pulls/dequeues and processes

**Schema Validator**

* Validate object shape before DB writes; reject with actionable message

**Rate‑Limited Fetch**

* Token bucket in Code node; retries with exponential backoff

---

## Performance & Reliability

* Use **batching** for API calls; respect rate limits.
* Parallelize CPU‑bound transforms via subworkflows.
* Make external calls idempotent; de‑dupe with stable keys.
* Prefer **pagination** and delta syncs over full scans.

---

## Troubleshooting & FAQs

**Q: Can we trigger a workflow execution via public API?**

* Use a **Webhook trigger** for explicit invocations and for CI smoke tests.

**Q: How do we prevent long webhook timeouts?**

* Acknowledge immediately (200 OK) and continue async via queue subworkflow.

**Q: Who can push/pull source‑controlled changes?**

* Instance owners/admins manage repo linkage; project admins may push; see runbooks.

**Q: How do we share credentials without exposing secrets?**

* Share at project or user level; values stay masked; rotate periodically.

---

## Appendix: Templates & Snippets

### A) Webhook smoke test (curl)

```bash
curl -sS -X POST "$STAGING_EDGAR_SMOKE_URL" \
  -H 'Content-Type: application/json' \
  -d '{"cik":"0000320193","form":"10-Q","limit":1}'
```

### B) Postgres upsert (Code node → SQL node)

```sql
INSERT INTO warehouse.company_events (cik, filed_at, form, hash, payload)
VALUES (:cik, :filed_at, :form, :hash, :payload::jsonb)
ON CONFLICT (hash) DO NOTHING;
```

### C) Error Router payload example

```json
{
  "run_id": "${$json.runId}",
  "workflow": "edgar_10q_monitor",
  "project": "Intelligence",
  "env": "staging",
  "err": {
    "message": "${$json.error.message}",
    "stack": "${$json.error.stack}"
  }
}
```

### D) JSON Schema (company event)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "CompanyEvent",
  "type": "object",
  "required": ["cik", "filed_at", "form", "source"],
  "properties": {
    "cik": { "type": "string", "pattern": "^\\d{10}$" },
    "filed_at": { "type": "string", "format": "date-time" },
    "form": { "type": "string" },
    "source": { "type": "string", "enum": ["edgar", "patents", "news"] },
    "payload": { "type": "object" }
  }
}
```

### E) Feature flag pattern (env/\*.env.json)

```json
{
  "limits": { "edgar": { "daily": 500 } },
  "flags": { "use_new_parser": true }
}
```

---

## Maintainers

* Mycroft Core Team — @maintainers

## License

* Apache‑2.0 (repository); vendor licenses apply to hosted products and nodes.

