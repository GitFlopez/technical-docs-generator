# Prompt 1: README Generator

## Purpose
Generate a complete, professional GitHub/GitLab README that makes developers want to use your project.

---

## The Prompt

```
You are a senior technical writer specializing in open-source and SaaS developer documentation.

Generate a complete GitHub README.md for the following project:

**Project Name:** [PROJECT_NAME]
**One-line description:** [DESCRIPTION]
**Target audience:** [developers / end users / both]
**Tech stack:** [LANGUAGE, FRAMEWORK, KEY DEPENDENCIES]
**Core features (list 4-6):**
[FEATURE_1]
[FEATURE_2]
[FEATURE_3]
[FEATURE_4]

**Installation command:** [npm install / pip install / etc.]
**Quick-start code example:**
[PASTE A SHORT CODE EXAMPLE OR DESCRIBE WHAT IT DOES]

**GitHub repo URL (if known):** [URL or leave blank]
**License:** [MIT / Apache 2.0 / GPL / etc.]

---

Generate a README.md with these sections, using proper Markdown formatting:

1. **Header** — project name as H1, one-line description, and 3-5 badge placeholders (build status, version, license, coverage, downloads — use shields.io placeholder format)

2. **Features** — H2 section, 4-6 bullet points with bold feature names and 1-sentence descriptions

3. **Installation** — H2 section, prerequisites (Node.js version, Python version, etc.), then installation command in a code block, then any required environment variable setup (.env example)

4. **Quick Start** — H2 section, minimal working example in a properly fenced code block with language tag, plus 2-3 sentences explaining what the example does

5. **Usage** — H2 section, 3-5 common use cases with code examples, including parameter descriptions in a table format

6. **Configuration** — H2 section, all config options in a Markdown table with columns: Option | Type | Default | Description

7. **API Reference** — H2 section, brief listing of main methods/endpoints with one-line descriptions (note: "See full API docs at [link]" placeholder)

8. **Contributing** — H2 section, fork/clone/branch/PR workflow, code style note, and how to run tests

9. **License** — H2 section, license name with link to LICENSE file

Rules:
- Use real-looking version numbers and badge URLs
- All code blocks must have correct language identifiers (javascript, python, bash, json, etc.)
- Write in direct, developer-friendly tone — no marketing language in the README
- Configuration table must be complete with realistic defaults
- Quick Start example must actually work (no pseudocode unless product doesn't exist yet)
- Target length: 400-600 lines of Markdown
```

---

## How to Use

1. Fill in all `[BRACKETED]` fields with your project's details
2. Paste the completed prompt into Claude
3. Copy the output into your repo's README.md
4. Replace badge placeholder URLs with real CI/CD and registry links
5. Insert actual screenshots where placeholders appear

## Tips

- The more detail you provide in the quick-start code example, the better the Usage section will be
- If your project has a CLI, describe the top 5 commands
- If it's an API library, describe the 3 most common method calls
- Run `markdownlint README.md` after to catch formatting issues
