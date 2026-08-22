# Eval Criteria & Prompt Iteration

This is the part of AI product work that doesn't show up in a typical PRD: how do we know the model is actually good enough to ship, and how do we improve it deliberately rather than by vibes? This doc lays out the golden set, the rubric, and how the v0 → v1 prompt logic evolved.

## 1. Golden set

A small, hand-picked set of representative tickets, each with an expected category and urgency label. In a real rollout this set grows from anonymized production tickets and disagreements between the model and agents; here it's hand-authored to cover the main patterns TriageIQ needs to handle.

| # | Ticket (abridged) | Expected category | Expected urgency |
|---|---|---|---|
| 1 | "I was charged twice for my subscription this month" | Billing | Medium |
| 2 | "The app crashes every time I open the reports tab" | Bug Report | High |
| 3 | "How do I export my data to CSV?" | How-To | Low |
| 4 | "I can't log in, it says my account is locked" | Account Access | High |
| 5 | "Would be great if you added dark mode" | Feature Request | Low |
| 6 | "URGENT — production data is missing since this morning, my whole team is blocked" | Bug Report | High |
| 7 | "Can I get a refund for last month?" | Billing | Medium |
| 8 | "Where do I find the API documentation?" | How-To | Low |
| 9 | "My password reset email never arrived" | Account Access | Medium |
| 10 | "This is the third time I've asked — still no response, extremely frustrated" | Other | High |
| 11 | "Just wanted to say the new dashboard is great, thanks!" | Other | Low |
| 12 | "Getting a 500 error when I try to invite teammates" | Bug Report | High |

## 2. Scoring rubric

Each model output is scored against the golden set on three axes:

- **Category accuracy** — does the predicted category match expected? (binary)
- **Urgency accuracy** — does predicted urgency match expected, or is it off by one level (acceptable) vs. two (fail)?
- **Reply quality** (manual review, 1–5 scale) — is the tone appropriate, does it avoid overpromising or fabricating details (e.g. inventing a refund policy), and is it usable as a starting draft with light editing?

A ticket only counts as a "pass" if category accuracy AND urgency accuracy (within one level) are both satisfied.

## 3. Prompt v1 → v2

**v1 (baseline):** Classify by simple keyword match alone (e.g. "refund"/"charged" → Billing), with urgency driven only by exclamation marks and the word "urgent."

- Result on golden set: 8/12 category pass, urgency correct on 6/12.
- Failure pattern: tickets #10 and #11 (no clear keyword signal, sentiment-driven) were both mis-classified as "How-To" by default; #6 was flagged Medium urgency because it lacked the literal word "urgent" despite clearly time-critical language ("production data is missing," "whole team is blocked").

**v2 (current):** Added sentiment/urgency-language signals (frustration phrases, business-impact phrases like "blocked," "production," "can't work") as a second signal alongside keyword category matching, and added a fallback "Other" category for tickets with no strong topical match instead of defaulting to "How-To."

- Result on golden set: 11/12 category pass, urgency correct (within one level) on 11/12.
- Remaining gap: ticket #9 ("password reset email never arrived") sits between Account Access and a possible Bug Report reading — flagged as a case to keep in the golden set and watch as the set grows, rather than over-fit a single rule to it.

## 4. What this demonstrates

The `/demo` prototype in this repo runs the **v2** logic. The takeaway for a real LLM-backed v1 (see [roadmap.md](roadmap.md)) is the same discipline at higher fidelity: maintain a growing golden set from real tickets, score category/urgency/reply-quality separately, and treat every agent override as a labeled data point for the next iteration — not just a UI bug to shrug off.
