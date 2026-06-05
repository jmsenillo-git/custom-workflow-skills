---
name: task-plan-creator
description: Revises raw task plans or creates new ones for Shopify development work (sections, themes, JS, CSS, audits, performance optimization, app testing) AND for study/learning plans (Claude skills, frameworks, new technologies). Always use this skill when the user says "improve this plan", "create me a plan", "plan this out", "draft a plan for", "task plan for", "study plan for", or "learning plan for" — even if the request looks simple. Also trigger it whenever the user shares a rough plan or task description and asks for it to be sharpened, sequenced, or scoped, even without those exact phrases. The skill scales output rigor to task weight (quick fix vs. standard feature vs. complex build vs. audit; light review vs. standard learning vs. deep learning), challenges the plan's premise before optimizing, and bakes in Shopify-specific checks (theme editor compatibility, JSON templates, app conflicts, mobile-first, a11y, LCP/CLS/INP, JS pitfalls).
---

# Task Plan Creator

A skill for turning rough task plans into sequenced, weight-appropriate, risk-aware plans for Shopify development work and for learning new technologies.

The user is a Shopify theme developer. Strong in Liquid, HTML, CSS, schema, metaobjects, sections. Actively learning JavaScript — treat JS as a learning opportunity, not just a deliverable. Values maintainability over clever shortcuts.

## Core principle

Never just polish a plan. Challenge the premise first, classify the weight, then scale the output to match. A 30-minute CSS tweak does not get the same ceremony as a multi-day section build with metaobjects and JS. Over-formatting small tasks wastes the user's time; under-formatting big tasks ships broken code.

## Process

Follow these steps in order.

### Step 1 — Detect plan type

Decide whether this is a **build plan** or a **study plan**.

- **Build plan** — the user wants to ship something: a Shopify section, a JS feature, a CSS refactor, an audit, a performance pass, an app test, a bug fix.
- **Study plan** — the user wants to *learn* something: a new tool, a framework, a concept, a Claude feature, a JS topic, an API. The output is knowledge, not code.

If genuinely ambiguous, ask one focused question. Otherwise, infer and state the assumption inline.

### Step 2 — Challenge the premise

Before optimizing the plan, ask whether the plan is solving the right problem. Only include this section in the output if there is something real to challenge — do not invent objections to look thorough.

For build plans, look for:

- Solving the wrong problem (rebuilding the cart drawer when one event listener is broken).
- Reinventing a solved problem (custom subscription logic instead of a subscription app; custom reviews instead of Judge.me/Yotpo; custom bundling for anything non-trivial).
- App vs. native mismatch (building native when an app is the right call, or reaching for an app when Shopify can do it natively).
- Overengineering (metaobject-driven system for a one-time banner).
- Underengineering (hardcoding what should be schema-driven).
- Sequencing that will cause rework (building UI before data model).

For study plans, look for:

- Wrong thing to learn now (learning Hydrogen when the user works in Liquid themes).
- Missing prerequisites (learning React without comfortable JS fundamentals).
- Overkill for the stated goal (deep-diving GraphQL when they just need one Storefront API query).
- Better adjacent topic (wanting to learn "JS frameworks" when the real need is vanilla JS fluency — a known weak spot).

Keep the challenge to a short paragraph. State the concern, propose the better path, then proceed with the plan they asked for unless they want to redirect.

### Step 3 — Classify weight

Pick the lightest tier that honestly fits. When in doubt, go lighter — the user can always ask for more depth.

**Build plan tiers:**

- **Quick fix** — under ~1 hour. Single file or single concern. No JS or trivial JS. Example: fix a CSS spacing bug, adjust schema setting, swap a Liquid filter.
- **Standard feature** — half-day to one day. Multiple files or moderate JS. Example: add a new section with 3-5 schema settings, build a product-card variant, add a filter dropdown.
- **Complex build** — multi-day or JS-heavy. Multiple sections/snippets, AJAX, state, or metaobject-driven content. Example: custom upsell logic in the cart drawer, metaobject-driven landing page system, variant picker rebuild.
- **Audit** — investigative work across the store/theme. Example: full speed audit, accessibility review, CRO review of the PDP, theme cleanup.

**Study plan tiers:**

- **Light review** — refreshing something the user has used before. A few key concepts to revisit and one quick exercise to confirm recall.
- **Standard learning** — new tool in a familiar paradigm. Example: learning a new Shopify app's API, picking up a new Liquid feature, learning a CSS technique.
- **Deep learning** — new paradigm or significant JS topic. Example: async/await and Promises, the Fetch API and `/cart.js`, event delegation, Web Components, Claude skills architecture, GraphQL fundamentals.

### Step 4 — Produce the plan, scaled to weight

Use exactly the structure for the tier. Do not add sections that do not apply. Do not omit sections that do.

#### Build plan — Quick fix

```
**Objective:** [one sentence]

**Steps:**
1. ...
2. ...
3. ...

**Risk:** [one line, only if there is a real risk — otherwise omit]
```

#### Build plan — Standard feature

```
**Objective:** [one sentence — what shipped looks like]

**Approach:** [2-3 sentences — the chosen path and a one-line "why not the alternative" if relevant]

**Files affected:**
- `path/to/file.liquid` — [what changes]
- `path/to/section.liquid` — [what changes]

**Steps:** [ordered, with the dependency reason called out where it matters]
1. ...
2. ...

**Risks & edge cases:**
- ...

**QA checklist:**
- Mobile (375px and up)
- Theme editor (settings save, section preview, drag/drop)
- [other items that matter for this specific task]
```

#### Build plan — Complex build

```
**Objective:** [what shipped looks like, including the user-facing outcome]

**Approach:** [the chosen architecture, and a short "why this over X" — at least one alternative considered]

**Files affected:**
- `path/to/file` — [purpose]
- ...

**JS concepts involved:** [only if JS is in the plan — name them so the user knows what to study/review before starting: e.g. event delegation, async/await, Fetch API, debouncing, DOM traversal, `this` binding]

**Phased steps:** [grouped by phase, with dependencies between phases called out]

Phase 1 — [name]:
1. ...
2. ...

Phase 2 — [name]:
1. ...

**Risks & edge cases:** [Shopify platform limits, app conflicts, race conditions, cart state desync, theme editor breakage, JSON template implications, mobile-specific issues]

**Performance / CRO / SEO impact:** [only the dimensions that actually matter for this task]

**QA checklist:**
- Mobile (375px and up)
- Accessibility (keyboard nav, focus states, screen reader labels)
- Theme editor compatibility
- App conflicts (name the specific apps to test against based on context)
- Core Web Vitals if relevant
```

#### Build plan — Audit

```
**Scope:** [what is and is not being audited]

**Methodology:** [how the audit will be performed — tools, manual checks, devices, sample pages]

**Deliverables ranked by impact:** [the audit will produce these]
1. [highest-impact category — e.g. LCP issues on PDP] — why it matters
2. ...
```

#### Study plan — Light review

```
**Objective:** [what the user wants to refresh]

**Concepts to revisit:**
- ...
- ...

**Quick recall exercise:** [a single concrete thing to do — explain X out loud, write a small snippet, etc.]
```

#### Study plan — Standard learning

```
**Objective:** [what the user wants to be able to do after]

**Prerequisites check:** [if any — flag if a prereq looks missing]

**Concepts in order:**
1. ...
2. ...

**Resources:** [official docs first, then reputable secondary sources — name them specifically, not "search online"]

**Hands-on exercise:** [one concrete thing to build/do that proves the concept stuck]

**You've learned it when:** [a clear, testable checkpoint — not "you feel comfortable"]
```

#### Study plan — Deep learning

```
**Objective:** [what the user wants to be able to do, applied to their real Shopify work]

**Prerequisites:** [what must be in place first]

**Phased roadmap:**

Phase 1 — Foundations:
- Concepts: ...
- Resources: ...
- Practice: ...

Phase 2 — Intermediate:
- ...

Phase 3 — Applied (Shopify integration):
- How this concept shows up in the user's real work (cart drawer, AJAX cart, variant pickers, metaobject rendering, etc.)
- A small project that integrates it

**Common pitfalls and misconceptions:** [things people get wrong when learning this]

**Self-test questions:** [3-5 questions the user should be able to answer in their own words]

**You've learned it when:** [testable checkpoint]

**Realistic time budget:** [days/weeks, accounting for the fact that the user has a job — no "8 hours a day" fantasies]
```

## Built-in Shopify checks (build plans)

For every build plan above Quick fix, mentally walk through these. Only surface the ones that actually apply — do not list them all defensively.

- **Theme editor compatibility** — does the section still render in the editor? Do default blocks load? Do settings save without errors?
- **JSON template implications** — is this section addable to JSON templates? Does it have presets? Does the schema make sense for merchant configuration?
- **App conflicts** — cart drawer apps, upsell apps, subscription apps, review apps, and ESP popups frequently fight with custom JS on the cart and PDP. Name the likely culprits based on context.
- **Mobile-first** — assume mobile is the default canvas. Flag if a layout will break under 375px.
- **Accessibility** — semantic HTML, keyboard nav, focus states, ARIA only where native HTML is insufficient.
- **Core Web Vitals** — LCP (hero images, fonts, render-blocking JS), CLS (images without dimensions, late-loading content), INP (heavy JS handlers, debouncing).
- **Liquid render performance** — nested loops, `all_products` lookups, sections inside sections, large `unless`/`for` chains.
- **JS pitfalls** — event listener duplication on re-rendered cart drawer, cart state desync between Liquid and `/cart.js`, race conditions on rapid clicks, `this` binding in callbacks, memory leaks from uncleaned listeners.

## Built-in study plan considerations

- **Realistic time** — the user has a job. Calendar weeks, not contiguous hours.
- **Active over passive** — every plan ends in something the user does, not just reads.
- **Connect to real work** — every study plan ties the concept back to something the user actually builds in Shopify.
- **Lean into the JS weak spot** — when JS is involved, go slightly deeper. Name the concepts. Suggest reading code, not just docs.
- **Source quality order** — official docs > MDN / Shopify dev docs > reputable named sources (Josh Comeau, Kyle Simpson, web.dev, etc.) > random blog posts. Do not recommend "search YouTube" — name a specific resource or skip it.

## Output style

Match the user's preferred communication style: direct, no filler, no "great question," no hedging. Use the user's terminology — they work in Liquid, metaobjects, sections, schema, and cart drawers daily. Do not over-explain Shopify basics.

For JS specifically, over-explain. Name concepts. Mention pitfalls. The plan should leave the user slightly more capable, not just unblocked.

## Examples

### Example 1 — vague build request gets weight-classified down

Input: "Create me a plan to fix the wishlist icon not showing on mobile."

This is a Quick fix. Do not produce a complex-build structure for it.

Output:

```
**Objective:** Make the wishlist icon visible on mobile.

**Steps:**
1. Inspect the icon's container in DevTools on a mobile viewport — confirm whether it's `display: none`, off-screen, or rendered but invisible.
2. Check the responsive CSS for the header — likely a media query hiding it under a breakpoint.
3. Fix the rule (or move the icon into the mobile nav if that's the intended pattern).
4. Test in theme editor mobile preview + real device.

**Risk:** If the wishlist icon was hidden on purpose because the wishlist drawer doesn't work on mobile, fixing the icon will expose a broken drawer. Confirm the drawer works before un-hiding.
```

### Example 2 — overengineered plan gets challenged

Input: "Here's my plan: build a metaobject-driven system to manage the 'free shipping over $X' threshold across the site. Improve this plan."

The premise is wrong for the actual problem. Challenge first.

Output:

```
**Premise check:** A metaobject-driven system for a single shipping threshold is overengineered. Shopify Markets / Shipping settings already store the threshold, and you can read it directly in Liquid via `cart.cart_level_discount_applications` or by hardcoding it in a single theme setting. A metaobject is the right tool when you need merchant-editable structured content with relationships — one number doesn't qualify. Recommend: theme setting in `settings_schema.json`, referenced from a snippet that renders the progress bar. If you still want metaobject-driven, here's the plan anyway:

[... standard feature plan follows ...]
```

### Example 3 — study plan, deep tier

Input: "Create me a learning plan for async JavaScript."

This is Deep learning — new paradigm, weak spot, foundational for cart/AJAX work.

Output uses the full Deep learning structure, with Phase 3 explicitly tying async/await and the Fetch API to `/cart.js`, variant updates, and cart drawer refreshes.

## Reminders

- Lead with the core plan. Skip preamble.
- Do not list considerations defensively — only surface the ones that apply.
- When the plan involves JS, name the concepts at play so the user can build vocabulary.
- When pushing back on a premise, propose the better path before defaulting to the user's original request.
- The output is the plan, not a meta-discussion of the plan.