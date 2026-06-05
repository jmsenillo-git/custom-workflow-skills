---
name: shopify-app-theme-bridge
description: >-
  Diagnose and fix Shopify storefronts where a third-party app has replaced a
  native theme component (variant picker, add-to-cart, gallery, swatches) and
  broken a built-in theme behaviour as a side effect. Use this skill whenever a
  theme feature "used to work" or "works on other products but not this one,"
  whenever an app like Kaching Bundles, Bold, ReCharge, Globo, or any bundle /
  subscription / variant app is involved in a bug, whenever a variant change
  doesn't update the gallery / price / inventory, or whenever the user asks to
  re-connect, re-sync, or "make X talk to Y" on a product page. Also use it when
  the user wants a diagnose-first investigation plan before writing code, or
  asks for the reusable browser-console diagnostic snippets. Trigger this even
  if the user only describes the symptom ("picking a colour doesn't change the
  image") and hasn't named an app — a broken native behaviour on a PDP is the
  signal.
---

# Shopify App–Theme Bridge

Fix Shopify storefronts where a third-party app replaced a native theme
component and silently broke a built-in behaviour. The classic case: an app
swaps the theme's variant picker for its own widget, and the theme's
variant→gallery (or variant→price, variant→inventory) sync stops firing
because it only listens to the theme's *own* picker.

The fix is almost never a rebuild. The native behaviour still exists — it's
just no longer receiving its input. The job is to reverse-engineer how the app
signals change, then re-feed the theme's existing machinery.

## Core principle: diagnose before you code

The single most important rule. Never write the fix based on assumptions about
either the app's event API or the theme's internals. Both are undocumented or
customised. Guessing produces code that fails silently — a listener that never
fires, a selector that never matches, no error, an hour lost.

Every step below is gated: do not advance until the previous step's output is
confirmed. If the user pushes to skip ahead to code, push back once and explain
why — a wrong assumption here is more expensive than the diagnostic.

## The phased workflow

Work through these in order. Each phase has a concrete exit condition.

### Phase 0 — Investigate how the app signals change

Find out *how* the app announces the change (variant selected, option picked).
Three possibilities, in order of how clean they are to hook:

1. The app fires a real DOM event on a real element — best case, just listen.
2. The app silently mutates the DOM or an input — hook a `MutationObserver`.
3. The app changes things with no event and no observable mutation — hook the
   app's own control (e.g. the click on its swatch) directly.

Give the user the diagnostic snippet from
`references/diagnostic-snippets.md` (the "detection" snippet). Have them run it
in the live page's DevTools console and click the app's control 3–4 times.

**Exit condition:** the console output identifies which of the three cases
applies, and you know the exact event name / element / selector to hook.

### Phase 1 — Track down the target and confirm the theme's API

Identify the theme and the native feature being re-bridged. Most paid themes
are Dawn-based — confirm it by the presence of Dawn's custom elements
(`media-gallery`, `variant-selects`, `product-info`, `slider-component`) and
script files (`media-gallery.js`, `product-form.js`). Dawn-based means the
native feature usually exposes a reusable method (e.g. `setActiveMedia()`) —
which is what keeps the fix small.

Confirm, live in the browser, never by assumption:
- The exact element and method the fix will call.
- The exact ID / data-attribute format the theme uses to label things (so the
  bridge's lookup produces matching values).
- Where the app's widget sits in the layout (decides mobile behaviour).

Read the theme's section file and the relevant snippet to confirm the data
shape. Use the "target confirmation" snippet in
`references/diagnostic-snippets.md`.

**Exit condition:** every selector, method name, and ID format the fix depends
on is confirmed from live output or theme source — zero guesses remain.

### Phase 2 — Build the data lookup

The bridge needs to translate the app's signal (often a human-readable name
like a colour) into the theme's identifier (an internal ID). Build this lookup
in Liquid so it ships in the HTML — no runtime fetch.

Render it as a JSON `<script type="application/json">` block, keyed by whatever
the app exposes, valued by the theme's identifier built the *exact* same way
the theme builds it.

**Exit condition:** the lookup's values match the theme's live identifiers
(spot-check one in the console).

### Phase 3 — Build the bridge

Write one self-contained snippet. It should:
- Listen for the app's signal (via the mechanism confirmed in Phase 0).
- Resolve the signal through the Phase 2 lookup.
- Call the theme's *existing* native method — never reimplement the behaviour.
- Apply device-specific behaviour only where the layout demands it.

See `references/implementation-guide.md` for the required structure, the
non-negotiable safeguards (idempotency, debounce, event delegation, `this`
binding, graceful failure), and a full annotated reference implementation.

**Exit condition:** the snippet is written, placed correctly in the section
file, and every safeguard from the implementation guide is present.

### Phase 4 — QA and harden

Test the matrix in `references/implementation-guide.md`. The two failure modes
that hide in QA: behaviour that double-fires after a theme-editor re-render
(idempotency), and the native handler still reacting in parallel (double sync).

**Exit condition:** all QA matrix rows pass on real mobile and desktop.

## Communication rules

This skill is used by developers who think in Liquid, sections, and schema but
are often weaker in JavaScript. Apply these:

- **Over-explain the JavaScript.** For any JS, name the concepts in play
  (event delegation, debouncing, `this` binding, `MutationObserver`, capture
  phase) and explain *why* the code is structured that way, not just what it
  does. Treat every JS task as a teaching moment.
- **Under-explain the Liquid and Shopify architecture.** The user knows it.
- **Lead with the cause, then the fix.** Tell the user what broke the native
  behaviour so they understand the system, not just the patch.
- **Always state where code goes** — file path, snippet vs section, and what
  it depends on.
- **Surface the app-dependency trade-off.** This kind of fix exists only
  because an app broke something native. Say so plainly, and note that if the
  app is removed the bridge should be deleted and native behaviour resumes.

## Deliverables

A finished task usually produces, in order:

1. The diagnostic findings (which detection case, confirmed selectors).
2. The bridge snippet + the one-line section edit.
3. A plain-language summary and/or task report — see
   `references/report-templates.md` for the layman-summary and full-report
   templates. Use these whenever the user asks for a summary, report, or
   write-up for a non-technical lead.

## Reference files

- `references/diagnostic-snippets.md` — Copy-paste browser-console snippets for
  Phase 0 (detection) and Phase 1 (target confirmation), plus a blank
  adaptable template. Read this at Phase 0.
- `references/implementation-guide.md` — Required bridge structure, the
  mandatory safeguards with rationale, a full annotated reference
  implementation, and the QA matrix. Read this at Phase 3.
- `references/report-templates.md` — Layman-summary and full task-report
  templates in plain language. Read this when producing deliverables.