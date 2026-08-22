# Roadmap

## v0 — Concept prototype (this repo)
**Goal:** validate the interaction model and classification/reply concept with stakeholders before investing engineering effort.

- Rule-based "AI" simulation (see [eval-criteria.md](eval-criteria.md)) driving a clickable UI (`/demo/index.html`)
- No real model, no backend, no integration — deliberately cheap to build and change
- Used to pressure-test the PRD's requirements against a tangible artifact, and to get early agent/stakeholder reactions before committing to v1 scope

## v1 — Real LLM integration
**Goal:** replace the rule-based logic with an actual model behind the same interface, in a controlled pilot.

- Swap the simulated classifier/drafter for a real LLM call, prompted using the v2 logic and golden set from [eval-criteria.md](eval-criteria.md) as the starting eval baseline
- Add cost and latency budgets per ticket (classification + draft should stay fast enough not to slow an agent down)
- Add PII handling review before any real ticket data touches the model
- Pilot with a single support team; track draft acceptance rate and categorization accuracy against the v0 targets in [PRD.md](PRD.md)
- Expand the golden set using real (anonymized) tickets and agent overrides collected during the pilot

## v2 — Personalization & feedback loop
**Goal:** make the tool improve itself and adapt to how each team actually works.

- Feed agent accept/edit/reject actions back into prompt iteration (closing the loop described in eval-criteria.md)
- Per-team tone/style customization for drafts (e.g. more formal for enterprise accounts)
- A lightweight analytics view for team leads: ticket volume by category over time, draft acceptance trends, time-to-first-action — turning the tool's own usage data into the reporting signal the original PRD problem statement called for
- Explore CRM/helpdesk integration (Zendesk, Intercom, etc.) once the standalone tool has proven value
