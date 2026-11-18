# Flutter Anti-Pattern Checklist

Flutter-native translation of the "AI-generated UI" tells that design
skills like Hallmark/Impeccable/Unslop catalog for web. Check a screen
against every item below before deciding it's done.

## Color

1. **Default seed-color purple/indigo** (`Colors.deepPurple`, the default
   Material 3 seed) still in use, with no deliberate brand color chosen.
2. **Every surface the same color** — no tonal variation between card,
   background, and elevated surfaces (`surfaceContainerLow/High`, etc.
   from `ColorScheme`, not ignored).
3. **Low contrast text-on-background** — light grey text on white,
   white text on a light accent color. Check against WCAG AA (4.5:1 for
   body text) where feasible.
4. **Color used decoratively with no meaning** — e.g., a red icon next
   to a neutral action, or inconsistent use of the same color for both
   "success" and "brand accent".
5. **Hardcoded hex colors scattered across the file** instead of
   `Theme.of(context).colorScheme.*` or named tokens.

## Typography

6. **Only one or two font sizes on the whole screen** — no distinction
   between a screen title, a section header, a value, and a caption.
7. **Every piece of text the same weight** (`FontWeight.normal`
   everywhere, or `bold` everywhere).
8. **Currency/number columns not distinguished from labels** — in a
   billing/POS table, amounts should read differently (weight, size, or
   alignment) from the item name next to them.
9. **Text overflow unhandled** — no `overflow: TextOverflow.ellipsis`,
   `maxLines`, or `Expanded`/`Flexible` wrap on text that can grow (long
   product names, customer names).

## Layout & spacing

10. **Magic-number padding/margins** (`EdgeInsets.all(13)`,
    `SizedBox(height: 7)`) instead of a shared spacing scale.
11. **Centered-everything** — a form, a list, or a dashboard with every
    element centered and no visual anchor or asymmetry.
12. **Inconsistent card/button corner radius** across the same screen.
13. **Every card at the same elevation** regardless of importance (a
    total/summary card should read as more important than a line item).
14. **Dense data screens with no scan path** — in tables (billing,
    purchase history, inventory), nothing visually guides the eye to the
    important column (usually the amount/total).

## Interaction & state

15. **No empty state** — a list/cart/history screen that's just blank
    white space when there's no data, instead of a short message or icon.
16. **No loading state beyond a bare spinner** on data-heavy screens.
17. **Destructive actions not visually distinct** — a "Delete"/"Cancel"/"Clear"
    action using the same button style as a safe action. (Confirmation
    *dialogs* may already exist per the app's business-logic fixes — this
    item is about the button's own color/weight, not whether a dialog
    exists.)
18. **No micro-feedback on action completion** — a save/checkout that
    gives no visual confirmation beyond a toast that's easy to miss.
19. **Every button the same style** — primary, secondary, and tertiary
    actions on one screen all rendered as the same `ElevatedButton`.

## Icons & imagery

20. **Mixed icon styles** — some outlined, some filled, from different
    icon sets, on the same screen.
21. **Icon size inconsistent** relative to the text next to it.

## Motion

22. **No transition on state change** that would benefit from one (cart
    item added/removed, tab switch) — abrupt visual jumps.
23. **Overuse of animation** — everything bounces/fades when a plain,
    instant state change would read as more confident and professional.

---

Severity guidance for `audit`: items 3, 9, 17 are usually HIGH
(usability/accessibility). Items 1, 2, 6, 7, 11, 13, 19 are usually
MEDIUM (generic look, not broken). The rest are usually LOW.
