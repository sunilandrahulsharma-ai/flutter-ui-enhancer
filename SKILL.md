---
name: flutter-polish
description: Anti-AI-slop design skill for Flutter apps (Material 3). Use whenever the user asks to polish, redesign, audit, or critique a Flutter screen, make a screen "look better" or "less generic/default", fix visual hierarchy, spacing, typography, or color consistency, or improve the UI of a Windows desktop or mobile Flutter app. Make sure to use this skill whenever the user mentions a screen looking "AI-generated", "default Material", "boring", "inconsistent", or asks for a design pass on a Flutter widget tree — even if they don't say "design skill" explicitly. NOT for backend/business-logic changes and NOT for web (HTML/CSS/React) projects.
---

# Flutter Polish

A design-review and redesign skill for Flutter apps built on Material 3. It
exists because generic AI-written Flutter screens share the same tells as
AI-written web pages — just in Flutter's own vocabulary: default seed-color
purple, every card at the same elevation, every button the same style,
centered-everything layouts, no hierarchy between heading/body/caption text.

This skill never touches business logic, state management, or services. It
only touches the widget/visual layer: colors, spacing, typography,
elevation, motion, layout structure.

## Before you start: find or create the project's design tokens

Never invent ad-hoc colors, spacing numbers, or text styles per screen. The
app must have (or will get, once) ONE source of truth other screens can
reuse.

1. Search the project for an existing theme file — common names: `app_theme.dart`,
   `theme.dart`, `colors.dart`, `text_styles.dart`, `spacing.dart`,
   under `lib/theme/`, `lib/utils/`, `lib/constants/`.
2. If one exists, read it fully before touching any screen. Reuse its
   `ColorScheme`, `TextTheme`, and spacing constants. Do not create a
   second, competing set of tokens.
3. If none exists, the FIRST time this skill runs on the project, create a
   minimal `lib/theme/app_tokens.dart` with:
   - A spacing scale: `4, 8, 12, 16, 24, 32, 48` (as named constants, e.g.
     `AppSpacing.sm`, `.md`, `.lg`).
   - A `TextTheme` with at least 4 distinct weights/sizes for
     heading / subheading / body / caption — not all the same size.
   - Confirm the app's `ColorScheme` (from `ThemeData`) has a real seed
     color or custom scheme, not the Flutter-default purple, unless the
     user has explicitly chosen purple as brand color.
   Tell the user this file was created and why, in one line.
4. Record what you found/created in `.flutter-polish/project-tokens.md`
   (create this folder if absent) so the next run of this skill on this
   project doesn't have to re-discover it. Read this file first on every
   future run, before re-scanning the project.

## Verbs

| Invocation | What it does |
| --- | --- |
| `polish <screen file>` (default) | Full design pass on one screen. Reads the screen, finds violations from `references/anti-patterns.md`, fixes them using the project's tokens, runs `flutter analyze` to confirm no new errors. |
| `audit <screen file>` | Read-only. Scores the screen against the anti-pattern list, returns a table (violation, file:line, severity). No edits. |
| `critique <screen file>` | Read-only, UX-focused (not pattern-matching). One paragraph: what works, what doesn't, in plain language — for a screen the user wants a second opinion on, not a fix. |

If the user gives a bare instruction with no verb ("make the Billing
screen look better"), treat it as `polish`.

## Design flow for `polish`

1. **Scope check.** Confirm which single screen/file is in scope. If the
   user named a flow with several screens, ask which one to start with —
   do not polish multiple screens in one pass; the user has said they
   want to go screen by screen and verify each one.
2. **Read the screen fully** before changing anything. Note every widget
   that renders color, text, spacing, or elevation.
3. **Run the anti-pattern checklist** (`references/anti-patterns.md`)
   against what you read. List every match with file:line before editing
   — this becomes the audit output even inside a `polish` run.
4. **Fix only what's on the checklist**, using the project's tokens from
   `app_tokens.dart` (or the existing theme file). Do not restyle a
   widget that has no checklist violation just because you'd personally
   do it differently — minimum change, same discipline as the project's
   own "ponytail" code rule.
5. **Never touch**: `onPressed`/`onTap` logic, service calls, state
   variables, business rules (discounts, guards, validation), navigation
   targets. If a visual fix seems to require a logic change, stop and
   tell the user instead of making it.
6. **Verify no regression**: run `flutter analyze` (issue count must not
   go up, 0 new errors) and `flutter test` (pass count must not go down).
   A visual change should never break a widget test that checks for a
   specific widget/text — if it does, update the test to match the new
   (intentional) UI, and say so explicitly in the report.
7. **Report**: before/after for each fix (short description, not a full
   diff dump unless asked), analyze/test counts, and explicitly state
   which file(s) were touched. If this is a project using the `MEMORY.md`
   convention described elsewhere in the project, add a row there too.

## Severity for `audit`

- **HIGH** — breaks usability or accessibility (low contrast text,
  unreadable overflow, no distinction between a destructive and a safe
  action).
- **MEDIUM** — generic/inconsistent but not broken (default seed color,
  every card same elevation, no type hierarchy).
- **LOW** — cosmetic polish (icon style mismatch, uneven spacing by a
  few px).

## What this skill is not

- Not a web/HTML/CSS tool. If the user's project is Flutter Web and the
  rendered output is effectively a browser DOM, the same checklist still
  applies conceptually, but do not import or run any HTML/CSS-based
  slop-test tooling — it does not see Flutter's widget tree.
- Not a logic fixer. If `polish` surfaces a bug (e.g., a number that's
  wrong, not just styled badly), report it separately and do not fix it
  under this skill's scope.

See `references/anti-patterns.md` for the full checklist and
`references/material3-reference.md` for the token vocabulary this skill
expects a project to have.
