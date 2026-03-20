---
applyTo: "**/*.{njk,html},app/routes/*.js"
---

# NHS App – Supplementary Instructions (for GitHub Copilot)

**Purpose**  
These instructions supplement the NHS Prototype Kit guidance so Copilot reliably generates code for the **NHS App prototyping kit**, which extends the core kit with App‑specific layouts and components. [1](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/)[2](https://github.com/nhsuk/nhsapp-prototype)

---

## Sources Copilot should learn from (in priority order)

1. **This file** (NHS App specifics and guardrails).  
2. **NHS Prototype Kit instructions and Frontend guide** from your LLM pack:  
   - `nhs-prototype-kit-guide.instructions.md`  
   - `nhs-frontend-guide.instructions.md`  
   - `nhs-frontend-component-reference.md` (consult *only* for exact parameters/examples).  
   - `nhs-frontend-sass-reference.json` (consult only if needed).  
   Store these in `.github/instructions/` and reference them in `.github/copilot-instructions.md`. [3](https://github.com/edwardhorsford/NHS-LLM-documentation)

---

## Target stack and versions

- Base your suggestions on **NHS Prototype kit v7.x** (uses **NHS Frontend v10**, removes jQuery, updates template inheritance). [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)  
- The App prototype also includes **NHS App Frontend (nhsapp-frontend) v5.x** — the CSS/component library that adds App-specific components on top of NHS Frontend. This is what provides the App-specific components listed above. [9](https://github.com/nhsuk/nhsapp-frontend)  
- Assume **Node 22** compatibility where relevant (noted in kit 6.2.0; remains valid in 7.x). [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)

---

## Use NHS App components and layouts **first**

- When creating pages, **prefer NHS App–specific layouts/components**. If a component is not available in the App kit, fall back to standard NHS Frontend. The App prototype builds on the core kit **with added App frontend code** for App‑specific parts. [2](https://github.com/nhsuk/nhsapp-prototype)[1](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/)

### NHS App–specific components (use these before NHS Frontend equivalents)

These components exist in the NHS App design system and are **not** in standard NHS Frontend. Always reach for these first when the use case fits: [8](https://design-system.nhsapp.service.nhs.uk/components/)

| Component | Purpose |
|---|---|
| **Badge** | Notification indicator (unread count) on icons or cards. Use visually hidden text for screen readers. |
| **Bottom navigation** | Home / Services / Your health / Messages nav — **app context only** (see note below). |
| **Buttons** | App-specific button variants — check for differences from standard NHS Frontend buttons. |
| **Card links** | Primary navigation pattern in the App. Prefer over `nhsuk-card` for linking to services or deeper content. |
| **Header (web)** | App-specific web header — use instead of the standard NHS header when building the web version. |
| **Tag** | Status or label indicator. |
| **Timeline** | Chronological content (e.g. appointment history). |
| **Top navigation** | Fixed header navigation bar. |
| **Section heading** | Structured page sections with consistent visual treatment. |

> **App vs. web context:** Bottom navigation is only present in the **NHS App mobile context**. It is **not** used in the web browser version of the NHS App — those same links appear in an NHS.UK header instead. Bottom navigation is also hidden when external content opens in a browser overlay within the App. When prototyping, be explicit about which context you are targeting.

---

## Template inheritance (v7+)

- **Do:** `{% extends "prototype-kit-template.njk" %}`.  
- **Don’t:** extend legacy `template.html`.  
This change was introduced to make future updates easier from v6 and remains the default approach in v7. [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)

---

## JavaScript conventions

- **Do not use jQuery** – it is **not included in v7**. Use vanilla JS and NHS Frontend’s module approach/progressive enhancement. [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)

---

## Nunjucks filters you should use

- Use built‑in filters before creating new ones.  
- When outputting NHS numbers, use **`formatNhsNumber`** for correct spacing and readability (added in **v7.1.0**). [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)

---

## Extending the prototype: filters and locals

The App prototype provides two dedicated extension points — **use these rather than inventing alternatives**:

- **`app/filters.js`** — add custom Nunjucks filters here. Export a function that receives the Nunjucks `env` and attaches methods to a `filters` object. Do not add filters anywhere else.
- **`app/locals.js`** — set variables that should be available across all templates (e.g. environment flags, shared display data). This is the correct place for prototype-wide configuration.

---

## Routes, sessions and locals (Express)

- Follow the **canonical route pattern**:  
  - **GET** reads from `req.session.data` and renders a view.  
  - **POST** validates/updates `req.session.data`, then **redirects** (PRG pattern).  
- If integrating the kit into a larger Express app, you may initialise **middleware/filters/utilities** from the **`nhsuk-prototype-kit` npm package** (middleware configure/init utilities are available). [5](https://github.com/nhsuk/nhsuk-prototype-kit-package)

---

## Dev workflow

- Use the App prototype’s standard dev loop: **`npm run watch`**. Keep this as the default in any generated README/tasks and avoid inventing new commands. [2](https://github.com/nhsuk/nhsapp-prototype)

---

## Reset / clear data

- Provide a **Reset/Clear data** capability in admin/footer links and implement the corresponding route.  
  - The core kit introduced a **Reset data** feature in **v6.0.0**.  
  - The NHS App prototype repo includes **“clear data logic”** in recent commits.  
  Copilot should preserve or extend these patterns rather than creating ad‑hoc alternatives. [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)[2](https://github.com/nhsuk/nhsapp-prototype)

---

## Accessibility, realism and openness

- Generate markup that is **accessible**, **realistic**, and in line with the NHS service manual’s prototyping goals (screen reader support, voice control, magnification). [6](https://prototype-kit.service-manual.nhs.uk/)

---

## Collaboration & workflow (App team norms)

- **Where to read more / point teammates:**  
  - NHS App prototype – getting started, context, and support channels (#prototype-kit, #nhsapp-design-system). [1](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/)  
  - **Store code on GitHub** (NHS Digital org) and recommended repo naming for App prototypes. [7](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/github/)

---

## How Copilot should use the docs

1. Start with the **guides** (this file + the Prototype Kit/Frontend guides). Only consult the **component reference** when exact parameters/examples are needed. [3](https://github.com/edwardhorsford/NHS-LLM-documentation)  
2. Prefer **App‑specific** components/layouts, then fall back to **NHS Frontend**. [1](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/)[2](https://github.com/nhsuk/nhsapp-prototype)  
3. Keep code **v7+ compliant** (template inheritance, no jQuery, current filters). [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)