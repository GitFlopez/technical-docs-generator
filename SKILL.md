# Technical Documentation Generator

**Version:** 1.0.0  
**Category:** Developer Tools / Technical Writing  
**Pricing:** $29 one-time | $9.99/month  
**Author:** max_0x1

---

## What This Skill Does

Generates professional technical documentation for software products in four formats:

1. **README Generator** — full GitHub/GitLab README with badges, setup guide, API quick-start, and contributing section
2. **API Reference Docs** — structured endpoint documentation with request/response examples, error codes, and SDKs
3. **User Guide** — step-by-step end-user documentation with screenshots placeholders, FAQs, and troubleshooting
4. **Release Notes & Changelog** — version-by-version changelog following Keep a Changelog format + announcement post

---

## Who This Is For

- **SaaS founders** shipping v1.0 who need docs before launch
- **Solo developers** maintaining open-source libraries (README + API docs)
- **Startup teams** whose engineers hate writing docs
- **Technical writers** who need a fast first draft
- **Agencies** building client deliverables that require documentation

---

## Inputs Required

Each prompt asks for:
- Product/library name and one-line description
- Target audience (developers, end users, both)
- Tech stack and key dependencies
- Core features or API endpoints
- Code examples (optional but improves output quality)

---

## The 4 Prompts

### Prompt 1: README Generator
Produces a complete GitHub README including:
- Project title, badges (build status, version, license), and elevator pitch
- Features list with descriptions
- Installation and setup instructions (with code blocks)
- Quick-start example (working code snippet)
- Full API/CLI usage overview
- Configuration reference table
- Contributing guidelines
- License section

### Prompt 2: API Reference Docs
Produces structured API documentation including:
- Authentication section (API keys, OAuth, JWT)
- Base URL and versioning policy
- Endpoint table with method, path, description
- Full endpoint detail blocks: parameters, headers, request body schema, response body schema
- Request/response examples (cURL + JSON)
- Error code reference table with resolution guidance
- Rate limiting and pagination docs
- SDK quick-start snippets (Python, JavaScript, cURL)

### Prompt 3: User Guide
Produces end-user documentation including:
- Getting started section (account setup, first action)
- Feature walkthroughs with numbered steps
- Screenshot placeholder callouts (where to insert images)
- Keyboard shortcuts and tips section
- FAQ (10 common questions mapped to concise answers)
- Troubleshooting guide (symptoms → causes → fixes)
- Glossary of key terms

### Prompt 4: Release Notes & Changelog
Produces version documentation including:
- CHANGELOG.md following Keep a Changelog format (Added/Changed/Deprecated/Removed/Fixed/Security)
- 3 release versions formatted (current, previous, previous-previous)
- Public announcement post for each release (Twitter/X thread format + blog intro paragraph)
- Migration guide section (breaking changes + upgrade steps)

---

## Example Output

See `/example/` directory for a complete worked example:
- Product: **TasQueue** — background task queue API for Node.js
- All 4 prompts run end-to-end
- Ready to publish or use as a template

---

## Pricing Strategy

| Tier | Price | What You Get |
|------|-------|--------------|
| One-time | $29 | Lifetime access to all 4 prompts |
| Monthly | $9.99/month | Prompts + monthly updates as best practices evolve |
| Done-For-You | $97/project | Max runs all 4 prompts on your product and delivers formatted docs |

---

## License

MIT-0 — free to use, no attribution required.
