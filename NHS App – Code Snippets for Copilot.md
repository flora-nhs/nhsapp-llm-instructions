# NHS App – Code Snippets for Copilot

## 1) Page skeleton (Nunjucks, v7+ compliant)
{# Prefer App layouts/components where available #}
{% extends "prototype-kit-template.njk" %} {# v7+ base template #}  {# [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases) #}

{% block pageTitle %}Example page – NHS App prototype{% endblock %}

{% block content %}
  <div class="nhsuk-u-reading-width">
    <h1 class="nhsuk-heading-l">Example page</h1>
    <p class="nhsuk-body">Use NHS App components/layouts first; fall back to NHS Frontend only when needed.</p> {# [1](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/)[2](https://github.com/nhsuk/nhsapp-prototype) #}
  </div>
{% endblock %}

{% block scripts %}
  <script>
    // Vanilla JS only; no jQuery in kit v7
    // Initialise any behaviours here.
    document.addEventListener('DOMContentLoaded', () => {
      // Example: focus the main heading for screen readers if needed
      const h1 = document.querySelector('h1');
      if (h1) h1.setAttribute('tabindex', '-1');
    });
  </script> {# [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases) #}
{% endblock %}


## 2) Route patterns (Express + sessions)

/**
 * GET: render page with data from session
 * POST: validate/update session then redirect (PRG)
 */
import express from 'express';
const router = express.Router();

router.get('/example', (req, res) => {
  const data = req.session.data || {};
  res.render('example/index', { data });
});

router.post('/example', (req, res) => {
  const { someField } = req.body;
  req.session.data = { ...req.session.data, someField };
  return res.redirect('/example/confirm');
});

export default router;

// If integrating the kit package into a larger app, initialise middleware/utilities accordingly.
// (Available via the nhsuk-prototype-kit npm package.)  [5](https://github.com/nhsuk/nhsuk-prototype-kit-package)


## 3) Using built-in filters (NHS numbers)

{# Output an NHS number using the v7.1.0 filter #}
<p class="nhsuk-body">
  NHS number: {{ data.nhsNumber | formatNhsNumber }}
</p> {# Ensures correct spacing for readability/accessibility. [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases) #}


## 4) Reset / clear data link (footer)

{# Add in a prototype admin footer or similar #}
<ul class="nhsuk-footer__list">
  <li class="nhsuk-footer__list-item">
    <a class="nhsuk-footer__list-item-link" href="/prototype-admin/reset?returnPage={{ currentPage | urlencode }}">
      Reset data
    </a>
  </li>
</ul>
{# Core kit adds Reset data (v6.0.0); App repo also includes clear data logic. [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)[2](https://github.com/nhsuk/nhsapp-prototype) #}


## 5) Dev loop

# Use the standard App prototype dev command:
npm run watch
# (Avoid inventing alternatives; this is the expected local workflow.)  [2](https://github.com/nhsuk/nhsapp-prototype)


## 6) Prompting tips for teammates (to get better Copilot results)

- “Create a new page that **extends `prototype-kit-template.njk`** and uses the **NHS App layout** for [X].”  [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)[1](https://design-system.nhsapp.service.nhs.uk/get-started/nhsapp-prototype/)
- “Add a **POST** route that saves `[field]` to `req.session.data` and **redirects** to `/check-answers`.”  [5](https://github.com/nhsuk/nhsuk-prototype-kit-package)
- “Render an NHS number using the **`formatNhsNumber`** filter.”  [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)
- “Use **vanilla JS** for behaviour; don’t use jQuery.”  [4](https://github.com/nhsuk/nhsuk-prototype-kit/releases)