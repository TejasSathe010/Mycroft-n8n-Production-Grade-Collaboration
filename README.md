# Mycroft-n8n-Production-Grade-Collaboration

## n8n Plans & Collaboration Guide

## Overview

n8n is a flexible workflow automation tool that supports both **n8n Cloud** (hosted by n8n) and **Self-Hosted** deployments. Plans vary based on collaboration features, execution limits, auditability, and enterprise-grade needs.

---

##  Plan Summary Table

| Plan Type                 | Pricing (Cloud)       | Collaboration Features                                         | Best Use Cases                                            |
|---------------------------|------------------------|----------------------------------------------------------------|-----------------------------------------------------------|
| **Community Edition**     | Free (self-hosted)     | Basic use—no projects, no credential sharing, single user. :contentReference[oaicite:1]{index=1} | Solo devs or one-off automations without team needs.      |
| **Starter (Cloud)**       | US $24/mo (annually US $20) for 2.5K executions/month. :contentReference[oaicite:2]{index=2} | 1 shared project, unlimited users, 5 concurrent runs, forum support. :contentReference[oaicite:3]{index=3} | Teams trialing MIDI-scale automations with DevOps buffer. |
| **Pro (Cloud)**           | Contact Sales          | 3 shared projects, 20 concurrent runs, 7-day history, admin roles, search, global variables. :contentReference[oaicite:4]{index=4} | Small teams scaling features and collaboration.           |
| **Business (Self-Hosted)**| Contact Sales          | Git version control, environments (dev/staging/prod), SSO, LDAP, 6 projects, insights 30 days. :contentReference[oaicite:5]{index=5} | Growing orgs needing structured workflows and security.   |
| **Enterprise**            | Contact Sales (cloud or self-hosted) | Unlimited projects, 200+ concurrent runs, 365-day history, external secrets, log streaming, API key scoping, SLA support. :contentReference[oaicite:6]{index=6} | Regulated orgs, financial tech, high-compliance teams.    |

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
