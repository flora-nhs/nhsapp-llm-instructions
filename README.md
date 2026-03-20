# NHS App – Supplementary Instructions for GitHub Copilot

This repository contains a Copilot instruction file for use with the [NHS App prototyping kit](https://github.com/nhsuk/nhsapp-prototype). It supplements the broader [NHS LLM documentation](https://github.com/edwardhorsford/NHS-LLM-documentation) — which covers the core NHS Prototype Kit and NHS Frontend — with guidance that is specific to the NHS App.

---

## What it does

The instruction file tells Copilot about things that are **different or additional** in the NHS App kit compared to the standard NHS Prototype Kit. Without it, Copilot tends to generate standard NHS Frontend code and miss App-specific components, patterns, and conventions.

It covers:

- **NHS App-specific components** — a named inventory of components (Badge, Card links, Timeline, etc.) that exist in [nhsapp-frontend](https://github.com/nhsuk/nhsapp-frontend) but not in standard NHS Frontend, so Copilot reaches for the right ones first
- **App vs. web context** — clarifies that bottom navigation is app-only; the web version uses an NHS.UK header instead
- **Macros vs HTML** — NHS Frontend components have Nunjucks macros; NHS App components may not, so Copilot should check before inventing macro calls
- **JavaScript conventions** — no jQuery (not included in kit v7), vanilla JS only
- **Nunjucks filters** — the `formatNhsNumber` filter for correct NHS number display
- **Extension points** — `app/filters.js` and `app/locals.js` as the correct places to add custom filters and shared template variables
- **Routes** — POST/redirect (PRG) pattern; no rendering directly from POST handlers
- **Coding standards** — ES6+, no semicolons, Allman style, BEM SCSS, UK English, sentence case
- **Prototyping context** — pragmatism is acceptable; no need for production-grade quality or test coverage

---

## How to use it

This file is designed to work alongside the [NHS LLM documentation](https://github.com/edwardhorsford/NHS-LLM-documentation) files. Set that up first, then add this file.

### 1. Copy the instruction file into your prototype repo

Place `NHS App – Supplementary Instructions (.md` into `.github/instructions/` in your prototype repo.

> The `applyTo` front-matter at the top of the file means Copilot will automatically load it when you are editing `.njk`, `.html`, or `app/routes/*.js` files. No manual attachment needed.

### 2. Reference it in your repo-level Copilot instructions

In your `.github/copilot-instructions.md`, add a reference so Copilot knows the file exists for context that doesn't match the `applyTo` glob:

```markdown
For NHS App-specific components, patterns, and conventions, refer to:
- `.github/instructions/NHS App – Supplementary Instructions (.md`
```

### 3. That's it

Copilot will now apply NHS App guidance automatically when working on the right files.

---

## Relationship to the NHS LLM documentation

This file is **supplementary** — it does not replace the NHS LLM documentation. The intended priority order is:

1. This file (NHS App specifics)
2. `nhs-prototype-kit-guide.instructions.md` (core kit patterns)
3. `nhs-frontend-guide.instructions.md` (NHS Frontend conventions)
4. `nhs-frontend-component-reference.md` (component parameters and examples)

---

## Questions and suggestions

Please use [GitHub issues](../../issues) for questions or suggestions on how to improve the instructions.
