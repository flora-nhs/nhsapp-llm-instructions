---
applyTo: "**/*.{njk,html},app/routes/*.js"
---

# NHS App – Supplementary Instructions (for GitHub Copilot)

These instructions supplement the NHS Prototype Kit guidance. They focus on what is **different or additional** in the NHS App prototyping kit — which extends the core kit with App-specific components from [nhsapp-frontend](https://github.com/nhsuk/nhsapp-frontend).

---

## Prototyping context

This is a prototype codebase. Some pragmatism is fine:

- Aim for outward realism; internal implementation can be simplified
- No need for production-grade quality, backwards compatibility, or test coverage
- Prefer server-side rendering over client-side JavaScript

---

## Use NHS App components first

When creating pages, **prefer NHS App-specific components**. If a component does not exist in the App kit, fall back to standard NHS Frontend.

These components exist in the [NHS App design system](https://design-system.nhsapp.service.nhs.uk/components/) and are **not** in standard NHS Frontend:

| Component | Purpose |
|---|---|
| **Badge** | Notification indicator (unread count) on icons or cards. Use visually hidden text for screen readers. |
| **Bottom navigation** | Home / Services / Your health / Messages nav — app context only (see note below). |
| **Buttons** | App-specific button variants — check for differences from standard NHS Frontend buttons. |
| **Card links** | Primary navigation pattern in the App. Prefer over `nhsuk-card` for linking to services or deeper content. |
| **Header (web)** | App-specific web header — use instead of the standard NHS header when building the web version. |
| **Tag** | Status or label indicator. |
| **Timeline** | Chronological content (e.g. appointment history). |
| **Top navigation** | Fixed header navigation bar. |
| **Section heading** | Structured page sections with consistent visual treatment. |

> **App vs. web context:** Bottom navigation is only present in the **NHS App mobile context**. It is **not** used in the web browser version — those links appear in an NHS.UK header instead. Bottom navigation is also hidden when external content opens in a browser overlay. Be explicit about which context you are prototyping for.

### Macros vs HTML

- For **NHS Frontend components** (fallback): prefer Nunjucks macros over raw HTML.
- For **NHS App-specific components**: check the [component page](https://design-system.nhsapp.service.nhs.uk/components/) first. Use the Nunjucks example if one is provided; otherwise use the documented HTML. Do not invent macro calls — not all App components have macros.

---

## JavaScript

Do not use jQuery — it is not included in the kit. Use vanilla JS.

---

## Nunjucks filters

When outputting NHS numbers, use the **`formatNhsNumber`** filter for correct spacing and readability:

```njk
{{ data.nhsNumber | formatNhsNumber }}
```

---

## Extending the prototype

Use the dedicated extension points — do not invent alternatives:

- **`app/filters.js`** — add custom Nunjucks filters here.
- **`app/locals.js`** — set variables available across all templates (e.g. environment flags, shared data).

---

## Routes (Express)

Follow the POST/redirect pattern:

- **POST** validates/updates `req.session.data`, then **redirects** (PRG pattern). Do not render directly from a POST handler.

---

## Coding standards

**JavaScript**
- ES6+ with arrow functions
- No terminating semicolons
- Allman style — opening braces on a new line for conditions and functions
- 2-space indentation
- Descriptive variable names in plain English (e.g. `index` not `i`)

**Nunjucks**
- Use `elseif` not `elif`
- No trailing commas in macro calls

**SCSS**
- BEM classes with full class names — not nested expansions
- Semicolons required

**Content**
- UK English spelling
- Sentence case for all headings and user-facing text — first word and proper nouns only
