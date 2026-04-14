# Prompt 3: User Guide

## Purpose
Generate a complete end-user documentation guide with step-by-step walkthroughs, FAQs, and troubleshooting — written for non-technical users.

---

## The Prompt

```
You are a senior UX writer and technical documentation specialist who writes user guides for SaaS products.

Generate a complete user guide for the following product:

**Product Name:** [PRODUCT_NAME]
**Product type:** [SaaS web app / desktop app / mobile app / browser extension]
**One-line description:** [WHAT IT DOES]
**Target user:** [WHO USES IT — e.g., "small business owners with no technical background"]
**Primary use case:** [THE MAIN JOB USERS HIRE THIS PRODUCT FOR]

**Core features to document (list 4-6):**
[FEATURE_1 — brief description of what user can do]
[FEATURE_2 — brief description]
[FEATURE_3 — brief description]
[FEATURE_4 — brief description]

**Common user problems or questions:**
[PROBLEM_1]
[PROBLEM_2]
[PROBLEM_3]
[PROBLEM_4]

**Key terms users need to understand:**
[TERM_1 — definition]
[TERM_2 — definition]
[TERM_3 — definition]

---

Generate a user guide with these sections:

1. **Welcome & Overview** — H1 header with product name, what the product does in plain English (no jargon), who it's for, and a "what you'll be able to do after reading this guide" promise

2. **Getting Started** — step-by-step account creation / installation, with each step numbered and a [Screenshot placeholder: description of what screenshot shows]. Include first login / onboarding flow (5-8 steps). End with a "You're ready!" confirmation checkpoint.

3. **Feature Walkthroughs** — For EACH feature listed above:
   - H2 section with feature name
   - One-sentence "why you'd use this" intro
   - Numbered step-by-step instructions (write as if user is reading while doing it)
   - [Screenshot placeholder] after key steps
   - A "Pro tip" callout box at the end
   - An "Important" or "Note" callout for any gotchas

4. **Keyboard Shortcuts & Time Savers** — Table of keyboard shortcuts (if applicable) + 5 power-user tips for getting more out of the product

5. **Frequently Asked Questions** — 10 Q&A pairs. Each question should be a real question a user would type into Google ("How do I...?", "Why is my...?", "Can I...?"). Answers should be 2-4 sentences, direct and action-oriented.

6. **Troubleshooting** — For each common problem provided above:
   - Symptom (what the user sees/experiences)
   - Likely causes (1-3 possibilities)
   - Step-by-step fix for each cause
   - "If that doesn't work..." escalation path (contact support)

7. **Glossary** — Definition list for each key term provided, written in plain English without jargon (even when defining technical terms). Format: **Term** — definition.

8. **Getting Help** — Contact support section with: help center URL placeholder, email support placeholder, community forum placeholder (if applicable), and expected response times.

Rules:
- Write at a 7th-grade reading level — assume the user is smart but not technical
- Never use the word "simply" or "just" (implies it's easy when it might not be for the user)
- Every numbered step should start with a verb: "Click...", "Enter...", "Select...", "Navigate to..."
- Screenshot placeholders must describe EXACTLY what the screenshot should show
- FAQ questions must mirror how real users phrase problems (not how engineers think about them)
- Target length: 500-700 lines of Markdown
```

---

## How to Use

1. Fill in the feature descriptions from your product's actual feature set
2. For "common user problems", check your support tickets or app store reviews for real user language
3. Replace `[Screenshot placeholder]` callouts with actual screenshots before publishing
4. For multi-product suites, run this prompt once per major product area

## Tips

- User guides convert better to help center articles (Intercom, Zendesk, Notion) when each H2 section becomes its own article
- The FAQ section can be directly used to train an AI support chatbot
- Share draft with a non-technical person before publishing — if they get confused, the prompt needs more detail about that feature
