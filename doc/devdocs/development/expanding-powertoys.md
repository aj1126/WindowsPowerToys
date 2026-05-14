# 🧩 How to expand PowerToys

This guide explains how to safely expand PowerToys, whether you are:

- adding a brand-new utility, or
- extending an existing utility with new capabilities.

It is meant as a practical checklist for contributors and reviewers.

---

## 1. Decide the expansion type

### A) Add a new utility

Use this path when the feature should be independently enabled/disabled, has its own settings surface, and does not logically belong inside an existing utility.

Start with: [Creating a New PowerToy](new-powertoy.md)

### B) Extend an existing utility

Use this path when the feature naturally belongs in an existing module and should share its lifecycle, settings, and UX.

Start in:

- `src/modules/<module-name>/`
- `src/settings-ui/Settings.UI/`
- `src/settings-ui/Settings.UI.Library/`

---

## 2. Architectural checkpoints

Before coding, confirm:

1. **Module boundary**: keep feature logic inside one module unless there is a clear shared need.
2. **Shared code placement**: only add code to `src/common/` when multiple modules benefit from it.
3. **Settings contract**: setting names, defaults, and schema remain consistent across module interface, settings UI, and persisted JSON.
4. **Runner integration**: if adding a new module or entry points, verify runner registration and module loading paths.
5. **Policy/elevation impact**: account for GPO, elevation, and startup behavior when relevant.

See architecture details in:

- [Architecture Overview](../core/architecture.md)
- [Runner and System tray](../core/runner.md)
- [Settings](../core/settings/readme.md)

---

## 3. Implementation checklist

- Define the user scenario and expected behavior first.
- Reuse existing module patterns before introducing new abstractions.
- Keep changes atomic (one logical change per PR).
- Use localized strings for user-facing text.
- Keep hot paths lightweight (avoid noisy logging in high-frequency callbacks/hooks).
- Update module assets/resources only when needed.

---

## 4. Validation checklist

For any expansion, validate:

- Build succeeds for impacted projects/configurations.
- Existing behavior is not regressed.
- New behavior is covered by tests (or documented why tests are not added).
- Module enable/disable lifecycle still works.
- Settings persist, load, and apply correctly.
- If applicable: tray interactions, hotkeys, IPC, and startup behavior still work.

Testing references:

- [UI Testing](ui-tests.md)
- [Fuzzing Testing](../tools/fuzzingtesting.md)

---

## 5. Required documentation updates

When expanding functionality, update docs in the same PR:

1. Module developer docs under `doc/devdocs/modules/` (if behavior/architecture changed).
2. Relevant settings/architecture docs if contracts changed.
3. User-facing docs (handled by internal process when needed).

---

## 6. Common pitfalls

- Spreading module-specific logic into `src/common/` too early.
- Breaking settings compatibility when renaming properties.
- Updating one side of runner/settings IPC without the other.
- Forgetting installer/package integration for newly introduced binaries.
- Skipping multi-monitor or multi-device validation for utilities that require it.

---

## 7. Recommended starting points

- Contributor setup and build: [Developer docs home](../readme.md)
- New utility scaffold: [Creating a New PowerToy](new-powertoy.md)
- Coding standards: [Development Guidelines](guidelines.md), [Coding Style](style.md)

