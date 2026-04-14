# Prompt 4: Release Notes & Changelog

## Purpose
Generate professional changelogs following Keep a Changelog format, version-specific release notes, public announcement posts, and migration guides for breaking changes.

---

## The Prompt

```
You are a developer relations engineer and technical writer who writes release communications for software companies.

Generate a complete changelog and release notes package for the following product:

**Product Name:** [PRODUCT_NAME]
**Product type:** [library / SaaS API / web app / CLI tool]
**Current version:** [e.g., v2.3.0]
**Release type:** [major / minor / patch]

**Changes in this release (paste a rough list — messy notes are fine):**
[LIST EVERYTHING THAT CHANGED — new features, bug fixes, deprecations, performance improvements, breaking changes]

**Previous version:** [e.g., v2.2.1]
**Key changes in previous version (brief):** [SUMMARY]

**Version before that:** [e.g., v2.1.0]
**Key changes in that version (brief):** [SUMMARY]

**Breaking changes (if any):** [DESCRIBE WHAT BREAKS AND WHAT USERS NEED TO DO]
**Deprecated features (if any):** [WHAT'S BEING DEPRECATED AND THE REPLACEMENT]
**Target audience reading release notes:** [developers / end users / both]
**Company/project Twitter/X handle (if any):** [@handle]

---

Generate the following:

**Section 1: CHANGELOG.md Update**

Add the current version to an existing CHANGELOG.md following the Keep a Changelog format (https://keepachangelog.com):
- Header: `## [VERSION] - YYYY-MM-DD`
- Subsections (only include non-empty): Added, Changed, Deprecated, Removed, Fixed, Security
- Each item is a bullet point in past tense: "Added support for...", "Fixed an issue where..."
- Items should be in order of user impact (most impactful first)
- Include the full CHANGELOG.md with all 3 versions (current + 2 previous)
- Include the CHANGELOG.md header, "Unreleased" section, and link to GitHub compare URLs (use placeholder format)

**Section 2: Release Announcement (Developer Audience)**

Write a release announcement blog post intro (300-400 words):
- Title: "[Product] [Version] is here — [one-line summary of biggest change]"
- Opening: what's the most exciting thing about this release and why it matters
- H2 sections for each major feature/fix, with 1-2 sentences per item
- Code example showing the most important new feature (before/after if it's a changed API)
- Upgrade instructions (2-3 lines)
- Closing CTA: link to full docs, GitHub, and community

**Section 3: Twitter/X Announcement Thread**

Write a 5-tweet thread:
- Tweet 1: "🚀 [Product] [version] is out! Here's what's new:" (hook, under 280 chars)
- Tweet 2-4: One major feature or improvement per tweet, with a code snippet or emoji-led bullet list
- Tweet 5: Upgrade/install command + link to release notes

**Section 4: Migration Guide (only if breaking changes exist)**

If breaking changes were listed above, generate:
- H1: "Migrating from [PREV_VERSION] to [CURRENT_VERSION]"
- Breaking changes summary table: Change | Before | After | How to Update
- Step-by-step migration checklist (numbered)
- Code before/after examples for each breaking change
- "Upgrade automatically" script if applicable (or note that no automated migration exists)
- Common migration errors and how to fix them

Rules:
- CHANGELOG.md must follow Keep a Changelog format exactly — reviewers will check
- All dates must be ISO 8601 format: YYYY-MM-DD
- Semantic versioning rules: MAJOR = breaking, MINOR = new feature, PATCH = bug fix
- Release blog tone: excited but professional — no "we're thrilled to announce" clichés
- Twitter thread must fit within 280 characters per tweet (count carefully)
- Migration guide code examples must show REALISTIC before/after code (not pseudocode)
- Target length: 400-600 lines of Markdown total across all 4 sections
```

---

## How to Use

1. Copy your messy PR merge notes / git log into the "changes in this release" field
2. Run the prompt to get a formatted first draft
3. CHANGELOG.md section: paste directly above your previous `##` version entry
4. Blog post: use as the intro section — add screenshots/metrics from your actual release
5. Twitter thread: schedule using Buffer/Hootsuite for launch day

## Tips

- Run this prompt after every release, not just major ones — even patch notes matter to users
- The migration guide is the most valuable output for major versions — users will cite it for months
- Keep a running "Unreleased" section in your CHANGELOG.md and paste it into this prompt at release time
- For monorepos with multiple packages, run this prompt once per affected package
