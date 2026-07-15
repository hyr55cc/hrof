# Contributing | المساهمة

شكرًا لاهتمامك بالمساهمة! Thank you for contributing.

## Workflow
1. Fork the repo and create a branch: `feat/<name>` or `fix/<name>`.
2. Run `flutter analyze` and `flutter test` — both must pass.
3. Keep commits small and meaningful (`feat: add reveal-word hint`, `fix: RTL grid offset`).
4. Open a Pull Request using the template.

## Code style
- Clean Architecture layers must stay separated: `presentation` never imports `data` directly — only `domain` abstractions via Riverpod providers.
- All user-facing strings must be **Arabic only**.
- Document every public class and method.
- New gameplay logic requires unit tests.

## Adding levels
Levels live in `assets/data/levels.json` (offline) and in the Firestore `levels` collection (cloud). See README → "Level format".
