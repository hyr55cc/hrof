# Changelog

## [1.0.0] - 2026-07-15
### Added
- Core Arabic word-connect gameplay: circular letter wheel, drag-to-connect gesture, animated golden connection line.
- Crossword grid revealed word-by-word, fully RTL.
- Arabic normalization engine (أ/إ/آ/ٱ → ا, ى → ي, ة → ه, diacritics stripped).
- Dictionary validation with bonus-word system (every 20 bonus words → 100 coins).
- Coins economy + hint system (reveal letter, reveal word, shuffle, skip level).
- Daily reward with streak calendar (7-day cycle) stored in Hive.
- Level repository abstraction: local JSON source (offline) + Firestore source (cloud, admin can add levels without app updates).
- Light/Dark Arabic-first themes (Cairo typeface), particle celebration effects, haptics.
- Unit tests for normalization and game logic.
- GitHub CI workflow, issue/PR templates, contribution guide.
