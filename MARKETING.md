# Marketing & Revenue Strategy — Technical Documentation Generator

## Elevator Pitch

Every developer-facing product needs documentation. Almost no developer wants to write it. Technical writers charge $75-150/hour. This skill generates production-ready docs in 25 minutes for $29 — one time.

---

## Market Size

- **10M+ active GitHub repos** with README files — most are poor quality
- **2.5M+ public APIs** (ProgrammableWeb estimate) — most have inadequate docs
- **30M+ developers worldwide** — all need to document something eventually
- **213K+ SaaS companies** — all need user guides and release notes
- **$3.8B technical writing market** (US alone, 2025) — high-cost, high-friction

---

## Target Buyers

### Primary: Solo developers and small teams
- Pain: have to document their own code, hate doing it
- Trigger: shipping a new project, open-source release, client handoff
- Willingness to pay: $29 one-time is an impulse buy

### Secondary: Technical writers and freelancers
- Pain: client expects docs in 48 hours, first draft takes all day
- Trigger: new client project, tight deadline
- Willingness to pay: $29 saves 3+ billable hours at $50+/hr = obvious ROI

### Tertiary: Startup founders
- Pain: need docs before Product Hunt launch, no technical writer on staff
- Trigger: launch week, investor demo, developer integrations
- Willingness to pay: $97 DFY is cheap relative to $150+/hr consultant

---

## Competitive Analysis

| Competitor | Price | What They Do | Gap |
|------------|-------|-------------|-----|
| Human technical writer | $75-150/hr | Full docs service | 10-50x more expensive |
| Swimm.io | $39/user/month | Auto-docs from code | Requires codebase integration; no API reference or user guides |
| Mintlify | Free-$150+/month | Docs hosting + AI | Platform lock-in; doesn't generate content |
| readme.com | $99+/month | API docs platform | Platform, not generator |
| ChatGPT (generic) | Free-$20/month | Generic prompts | No structure, requires expertise to prompt correctly |
| **This skill** | **$29 one-time** | **Structured prompts for 4 doc types** | **Right format, right questions, right output** |

**Key differentiation:**
- 4 specialized prompts vs. one generic AI chat
- Structured inputs (you fill in a form) → structured outputs (markdown you can paste directly)
- No platform lock-in — outputs work in GitHub, Notion, GitBook, Mintlify, ReadTheDocs, Confluence
- Keep a Changelog format enforced in Release Notes prompt (developers will notice if this is wrong)
- API Reference prompt enforces OpenAPI-compatible structure

---

## Revenue Projections

### Base case: ClawHub installs

| Month | Installs | One-time ($29) | Subscriptions ($9.99) | Total |
|-------|----------|----------------|----------------------|-------|
| 1 | 20 | $580 | $0 | $580 |
| 3 | 60 | $1,740 | $120 | $1,860 |
| 6 | 120 | $3,480 | $480 | $3,960 |

### DFY upsell layer

- $97/project × 5 projects/month = $485/month
- 20 min of Max's time per project (4 prompts × 5 min each)
- Effective hourly rate: $291/hr

### Month 3 combined projection: ~$1,860 (installs) + $485 (DFY) = **$2,345/month**

---

## Why Developers Pay for This

Developers understand ROI math:
- A technical writer costs $600-1,200 per day
- This skill: $29 once, 25 minutes of their time
- The output quality matches a junior technical writer's first draft
- They tweak it, not write from scratch — that's the value

The "done-for-you" tier is for founders who value their time over money: "Give me your product, I'll give you docs."

---

## Portfolio Context

This is the first developer-tools skill in the portfolio. Previous 26 skills target marketers, writers, HR, and business operators. Technical Documentation Generator:

- Opens a new buyer persona (developers)
- Complements the existing skills (a developer who ships a SaaS product also needs landing page copy, email sequences, and case studies)
- Cross-sell opportunity: pitch the full portfolio to technical founders who need marketing content after their docs are done

---

## Launch Strategy

1. Publish to ClawHub free tier (MIT-0 license) — maximize install velocity
2. Post to Moltbook builds submolt — developer audience, good fit
3. Reddit outreach: r/programming, r/webdev, r/SideProject — "I built a set of prompts that generates docs, here's the output"
4. GitHub Gist: post README example as a real gist — drives organic discovery
5. DFY upsell in README footer and in Moltbook post replies

---

## Pricing Rationale

- **$29 one-time:** Developer impulse buy. Cheap enough to not think about, valuable enough to use.
- **$9.99/month:** For teams who want prompt updates as best practices evolve
- **$97 DFY:** For founders in launch mode who want docs delivered, not prompts to run

Keep the one-time price at $29 — don't raise it. The goal is installs and social proof, not margin. DFY is where the money is.
