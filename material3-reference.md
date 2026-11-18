# Material 3 Token Vocabulary (reference for `flutter-polish`)

Use this as the vocabulary when fixing items from `anti-patterns.md`. The
goal is to point the project at Flutter's own real tokens, not invent new
ones.

## Color — `ColorScheme`

Material 3's `ColorScheme` already has the roles most screens need. Use
`Theme.of(context).colorScheme.<role>` instead of hardcoded colors:

- `primary` / `onPrimary` — main brand action (e.g. Checkout button)
- `secondary` / `onSecondary` — supporting actions
- `error` / `onError` — destructive actions, validation messages
- `surface`, `surfaceContainerLow`, `surfaceContainerHigh` — layered
  surfaces for cards/sheets at different "importance" levels
- `outline`, `outlineVariant` — borders/dividers, not a hardcoded grey

If the app doesn't have a deliberate brand color yet, generate a
`ColorScheme.fromSeed(seedColor: <brand color>)` with a color the project
actually uses elsewhere (e.g. its logo/accent), not the Material default.

## Typography — `TextTheme`

Material 3's `TextTheme` has a built-in scale — use it rather than ad hoc
`TextStyle(fontSize: 14)` everywhere:

- `displayLarge/Medium/Small` — rarely needed outside splash/marketing
- `headlineLarge/Medium/Small` — screen titles
- `titleLarge/Medium/Small` — section headers, card titles
- `bodyLarge/Medium/Small` — main content text
- `labelLarge/Medium/Small` — buttons, chips, captions

A billing/POS screen typically needs: `titleMedium` for item names,
`bodyLarge` or a custom bold variant for amounts, `labelSmall` for
captions like "Stock: 20".

## Spacing scale

If the project has no spacing constants, use Material's common 4px-based
scale: `4, 8, 12, 16, 24, 32, 48, 64`. Name them (`AppSpacing.xs/sm/md/
lg/xl`) rather than repeating raw numbers.

## Elevation / surface tint

Material 3 prefers **surface tint** (subtle color shift) over heavy
drop-shadows for elevation. A `Card` with `elevation: 1` plus the right
`surfaceContainer*` color usually reads better than `elevation: 8` with a
dark shadow — especially on light backgrounds like a POS billing screen.

## Shape

`ThemeData.cardTheme`, `elevatedButtonTheme`, etc. can set one consistent
corner radius project-wide. Avoid per-widget `BorderRadius.circular(n)`
with a different `n` each time.
