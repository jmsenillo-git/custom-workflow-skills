# custom-workflow-skills

A personal collection of custom AI agent skills for Shopify theme development and self-directed learning, built by John Mark Senillo.

Each skill encodes a repeated, high-friction workflow into reusable procedural knowledge — so the agent handles it the same considered way every time, instead of being re-prompted from scratch. Skills are written for an agent that already knows Liquid, schema, metaobjects, and sections, and is actively building JavaScript fluency.

## Skills

| Skill | What it does |
|-------|--------------|
| [task-plan-creator](skills/task-plan-creator) | Turns rough task plans into sequenced, weight-aware, risk-checked plans for Shopify builds and for learning new tech. Challenges the premise before optimizing, and scales output rigor to the actual size of the task. |
| [shopify-app-theme-bridge](skills/shopify-app-theme-bridge) | Diagnoses and fixes storefronts where a third-party app has replaced a native theme component (variant picker, add-to-cart, gallery, swatches) and broken a built-in behaviour as a side effect. Diagnose-first: produces an investigation plan before writing code. |

## task-plan-creator

Takes a rough plan or task description and returns a plan that's scoped, sequenced, and risk-checked — without over-formatting small tasks or under-scoping large ones.

**How it works:**

- **Detects plan type** — a *build* plan (ship a section, JS feature, CSS refactor, audit, perf pass) or a *study* plan (learn a tool, API, concept, or JS topic).
- **Challenges the premise first** — flags overengineering, reinvented solutions (custom subscriptions/reviews/bundling that an app should own), app-vs-native mismatches, and sequencing that will cause rework. Only when there's something real to challenge.
- **Scales to task weight** — four build tiers (quick fix → standard feature → complex build → audit) and three study tiers (light review → standard learning → deep learning). The lightest honest tier wins.
- **Names the JavaScript concepts in play** — event delegation, async/await, Fetch API, debouncing, `this` binding, and so on — so JS work doubles as vocabulary-building.
- **Runs Shopify-specific checks** — theme editor compatibility, JSON template implications, app conflicts, mobile-first, accessibility, Core Web Vitals (LCP/CLS/INP), Liquid render performance, and common cart/AJAX JS pitfalls.

**Triggers on:** "improve this plan", "create me a plan", "plan this out", "draft a plan for", "task plan for", "study plan for", "learning plan for" — and on any rough plan shared with intent to sharpen it.

## shopify-app-theme-bridge

Diagnoses and fixes the specific class of storefront bug where a third-party app has overridden a native theme component on a product page and silently broken a built-in behaviour — a variant change that no longer updates the gallery, price, or inventory; a feature that works on other products but not this one.

**How it works:**

- **Diagnose-first** — produces a structured investigation plan before any code is written, isolating whether the break is app-side or theme-side.
- **Targets the common culprits** — bundle, subscription, and variant apps (Kaching Bundles, Bold, ReCharge, Globo, and similar) that replace the native variant picker, add-to-cart, gallery, or swatches.
- **Ships reusable browser-console diagnostic snippets** for reproducing and pinpointing the desync.

**Triggers on:** a native PDP behaviour that "used to work" or "works on other products but not this one"; any bug involving a bundle / subscription / variant app; a variant selection that doesn't update gallery / price / inventory; or a request to re-connect, re-sync, or "make X talk to Y" on a product page — even when the user only describes the symptom and hasn't named an app.

## Install

Install every skill in the repo:

```bash
npx skills add jmsenillo-git/custom-workflow-skills
```

Install a single skill:

```bash
npx skills add jmsenillo-git/custom-workflow-skills -s task-plan-creator
npx skills add jmsenillo-git/custom-workflow-skills -s shopify-app-theme-bridge
```

Skills install at project level by default. Browse and learn more at [skills.sh](https://skills.sh).

## Repo structure

Each skill is fully self-contained in its own folder under `skills/`, holding a `SKILL.md` plus any reference files or scripts it depends on. Nothing is shared across skills at the repo root — that keeps single-skill installs clean and lets each skill be versioned or removed independently.

```
custom-workflow-skills/
├── README.md
└── skills/
    ├── task-plan-creator/
    │   └── SKILL.md
    └── shopify-app-theme-bridge/
        └── SKILL.md
```

A skill is just a directory with a `SKILL.md` containing YAML frontmatter (`name` and `description`) followed by the instructions the agent loads when the skill triggers.

## License

MIT