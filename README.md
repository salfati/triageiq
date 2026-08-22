# TriageIQ

**An AI-assisted support ticket triage & draft-reply concept — PRD, eval design, and a clickable prototype.**

> Live demo: *(add your GitHub Pages link here once published — see "Publish this repo" below)*

## The problem

Support agents spend a large share of their day on two repetitive steps before they can actually help a customer: figuring out what kind of issue a ticket is, and drafting the first reply. Both are slow at volume and inconsistent across agents, which delays response times and makes team-level reporting unreliable.

## The idea

TriageIQ classifies an incoming ticket by category and urgency, and drafts a suggested first reply — with the agent always in the loop. Nothing is ever sent automatically; the agent accepts, edits, or rejects every draft.

**[Try the prototype →](demo/index.html)** (open locally, or via the live link above once this repo is published)

## My role

This project was scoped and written end-to-end as a product exercise:

- **Problem framing & requirements** — [docs/PRD.md](docs/PRD.md): problem statement, users, goals/non-goals, requirements, success metrics, risks
- **AI-specific rigor** — [docs/eval-criteria.md](docs/eval-criteria.md): a golden test set, a scoring rubric, and a worked example of iterating a prompt from v1 to v2 based on where it failed
- **Sequencing** — [docs/roadmap.md](docs/roadmap.md): how this concept prototype leads into a real LLM-backed pilot and then a feedback-driven v2
- **Prototype** — [demo/index.html](demo/index.html): a single-file, no-build, no-API-key clickable prototype used to pressure-test the concept before any real engineering investment

The prototype's "AI" logic is a deliberately simple rule-based simulation (clearly labeled in the UI) — the point of a v0 prototype like this is to validate the *interaction model and requirements* cheaply, before committing to a real model integration. That reasoning is explained in [docs/roadmap.md](docs/roadmap.md).

## Repo structure

```
triageiq/
├── README.md
├── docs/
│   ├── PRD.md              # problem, users, requirements, success metrics, risks
│   ├── eval-criteria.md    # golden set, rubric, prompt v1 → v2 iteration
│   └── roadmap.md          # v0 (this repo) → v1 real LLM → v2 feedback loop
└── demo/
    └── index.html          # the clickable prototype
```

## Running the prototype locally

No install, no build step, no API key required.

1. Download or clone this repo.
2. Open `demo/index.html` directly in any browser (double-click it, or drag it into a browser window).

## Publish this repo

To push this to your own GitHub and get a shareable live link for your resume/LinkedIn:

```bash
cd triageiq
git init
git add .
git commit -m "Initial commit: TriageIQ concept prototype"
git branch -M main
git remote add origin https://github.com/<your-username>/triageiq.git
git push -u origin main
```

Then, on GitHub: **Settings → Pages → Deploy from a branch → `main` / `/ (root)` → Save.** After a minute, GitHub gives you a live URL (something like `https://<your-username>.github.io/triageiq/demo/`) — drop that link into the README badge above and onto your resume.
