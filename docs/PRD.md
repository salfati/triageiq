# PRD: TriageIQ — AI-Assisted Support Ticket Triage & Draft Reply

**Owner:** Fathima Saleem (Product Manager) · **Status:** v0 prototype · **Last updated:** 2026-08-23

## 1. Problem

Support teams spend a large share of their day on two repetitive, low-judgment steps before they can actually help a customer: figuring out *what kind* of issue a ticket is (category, urgency) and drafting the first response. Both steps are time-consuming at volume, inconsistent across agents, and delay time-to-first-response — the metric most correlated with customer satisfaction on support tickets.

Manual triage doesn't scale linearly with ticket volume, and inconsistent categorization corrupts downstream reporting (teams can't reliably answer "what are customers actually contacting us about?").

## 2. Target users

- **Primary:** Frontline support agents, who currently read, classify, and draft a reply to every incoming ticket by hand.
- **Secondary:** Support team leads, who rely on accurate categorization for staffing, reporting, and identifying systemic product issues.

## 3. Goals

- Cut the time an agent spends reading and triaging a new ticket before they can act on it.
- Give agents a reasonable first-draft reply they can accept, edit, or discard — never one that sends automatically.
- Improve the consistency and reliability of ticket categorization for downstream reporting.

### Non-goals (for this version)

- Fully autonomous ticket resolution or auto-send of AI-drafted replies.
- Multi-language support.
- Deep CRM/helpdesk integration (Zendesk, Intercom, etc.) — this prototype is a standalone concept validator.

## 4. Requirements

| # | Requirement | Priority |
|---|---|---|
| 1 | Given ticket text, classify it into a category (Billing, Bug Report, How-To, Account Access, Feature Request, Other) | Must |
| 2 | Assign an urgency level (Low / Medium / High) based on ticket content and language signals | Must |
| 3 | Generate a draft reply in an appropriate tone for the category | Must |
| 4 | Agent can view the draft alongside the original ticket and edit freely before use — nothing is sent on the customer's behalf | Must |
| 5 | Surface *why* a classification was made (which signals/keywords triggered it), for agent trust and debuggability | Should |
| 6 | Track agent accept/edit/reject actions on drafts, to build a feedback signal for prompt iteration | Should |
| 7 | Support real LLM backends behind the same interface (see [roadmap](roadmap.md)) | Later |

## 5. Success metrics

- **Time-to-first-action per ticket** — target: reduce by 30–40% vs. manual triage baseline.
- **Draft acceptance rate** — % of AI drafts agents use with zero or minor edits. Target: >50% by v1.
- **Categorization accuracy** — measured against the golden set in [eval-criteria.md](eval-criteria.md). Target: ≥90% category match, ≥80% urgency match.
- **Agent trust score** — a simple post-rollout survey question ("I trust this tool's suggestions"), tracked over time as prompts improve.

## 6. Risks & mitigations

| Risk | Mitigation |
|---|---|
| AI drafts a factually wrong or inappropriate reply, agent sends it unedited | Draft is always a suggestion, never auto-sent; UI clearly labels drafts as AI-generated and requires an explicit agent action |
| Mis-classification skews team reporting | Track agent overrides of AI classification as a first-class metric, feed back into eval set |
| Over-reliance erodes agent judgment over time | Keep original ticket text primary in the UI; draft is a secondary panel, not the default view |
| Category/urgency model doesn't generalize across ticket types the team actually sees | Golden set is built from real (anonymized) ticket patterns and expanded over time, not written in a vacuum |

## 7. Out of scope risks (acknowledged, not solved here)

- PII handling / data residency for real customer ticket data — required before any production rollout, addressed at v1 (see roadmap).
- Cost and latency budgets for a real LLM backend — addressed at v1.

## 8. What this v0 prototype is

A **clickable, non-production prototype** (`/demo/index.html`) used to validate the interaction model and the classification/reply concept with stakeholders before investing in a real LLM integration. Its "AI" logic is a simple rule-based simulation, intentionally — see [roadmap.md](roadmap.md) for how it evolves.
