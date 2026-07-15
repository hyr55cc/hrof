<div dir="rtl">

# وصلة الكلمات — Arabic Word Puzzle 🕌✨

لعبة كلمات عربية أصلية بالكامل: صِل الحروف على العجلة الذهبية لتكوين كلمات عربية صحيحة وكشفها في شبكة الكلمات المتقاطعة. اللعبة عربية أولًا: واجهة عربية 100%، اتجاه من اليمين إلى اليسار، وخطوط عربية.

</div>

An original, Arabic-first word-connect puzzle game built with **Flutter + Riverpod + GoRouter + Hive**, with a **Firestore-ready** level pipeline. Clean Architecture, fully offline-capable, RTL throughout, zero English in the UI.

---

## ✅ What is implemented and working now

| Area | Status |
|---|---|
| Letter wheel with drag gesture + glowing golden connection line | ✅ |
| RTL crossword grid with reveal animations | ✅ |
| Arabic normalization (أ إ آ ٱ → ا · ى → ي · ة → ه · التشكيل ignored) | ✅ + unit tests |
| Dictionary validation & bonus words (every 20 → 100 coins) | ✅ |
| Coins economy + hints (reveal letter / reveal word / shuffle / skip) | ✅ |
| Daily reward: 7-day streak calendar (Hive) | ✅ |
| 5 hand-crafted sample levels (offline JSON) | ✅ |
| Level repository abstraction: **local JSON + Firestore datasource** | ✅ (Firestore code ready, activation below) |
| Dark / Light Arabic themes (Cairo typeface), glassmorphism, particles, haptics | ✅ |
| Splash, Home, Levels map, Game, Daily Reward, Settings screens | ✅ |
| CI (GitHub Actions), issue/PR templates, MIT license | ✅ |

## 🚧 What you must wire up yourself (honest scope notes)

These require **your accounts, credentials, or large data files** and cannot ship pre-made:

- **Firebase project** — run `flutterfire configure` (steps below). Firestore datasource, repository fallback logic and level schema are already written.
- **Full 300k+ dictionary** — the repo ships a curated ~500-word starter list. Drop any large open Arabic wordlist (one word per line, e.g. from the Arabic Wordlist / AraComLex / Tashaphyne open datasets) into `assets/data/dictionary_ar.txt`. No code changes needed — every entry is normalized at load time.
- **AdMob, In-App Purchases (StoreKit / Play Billing), FCM, Auth (Google/Apple), Leaderboards, Admin dashboard** — not included in this codebase yet. The architecture (Clean layers + repository interfaces + Riverpod DI) is designed so each of these plugs in as a new datasource/service without touching gameplay code.
- **Store publishing** — signing keys, bundle IDs, screenshots, privacy declarations are yours to create.

## 🚀 Getting started

```bash
git clone <your-repo-url> && cd arabic_word_puzzle

# Generate platform folders (not committed to keep the repo clean)
flutter create . --project-name arabic_word_puzzle

flutter pub get
flutter test
flutter run
```

Requires Flutter stable ≥ 3.27 / Dart ≥ 3.3.

## 🔥 Enabling Firebase (cloud levels)

1. `dart pub global activate flutterfire_cli`
2. `flutterfire configure` → generates `lib/firebase_options.dart` (git-ignored).
3. In `lib/main.dart`, uncomment the two Firebase lines.
4. In `lib/core/constants/app_constants.dart`, set `AppConfig.useFirebase = true`.
5. Create a Firestore collection **`levels`** — document id = level number, fields:

```json
{
  "id": 87,
  "difficulty": "متوسط",
  "letters": ["ا","ل","ع","خ","ت"],
  "rewardCoins": 30,
  "words": [
    { "word": "لعب", "row": 0, "col": 0, "dir": "h" },
    { "word": "لعل", "row": 0, "col": 0, "dir": "v" }
  ]
}
```

Admins add documents at any time → players get new levels **without an app update**. Bundled JSON remains the offline fallback.

Suggested security rules (read-only levels for clients):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{db}/documents {
    match /levels/{id} { allow read: if true; allow write: if false; }
  }
}
```

## 🗺️ Level format

- Grid is **RTL**: `col: 0` is the **rightmost** cell; horizontal words run right → left, vertical words top → bottom.
- `letters` are the wheel letters (duplicates allowed).
- Every answer must be buildable from the wheel letters, and intersecting cells must share the same letter (`test/level_entity_test.dart` shows how to verify).

## 🏗️ Architecture

```
lib/
├── core/            # theme, router, constants, Arabic normalizer
├── domain/          # pure entities + repository contracts (no Flutter/Firebase)
├── data/            # JSON & Firestore datasources, Hive storage, repo impls
└── presentation/    # Riverpod providers, game controller, screens, widgets
```

- `presentation` depends only on `domain` abstractions; concrete `data` classes are bound once in `providers.dart` / `main.dart` (dependency inversion).
- `GameController` (StateNotifier) owns every gameplay rule; widgets are pure renderers — easy to unit-test and to keep at 60 FPS (const widgets, CustomPainter for the wheel, no per-frame allocations).

## 🧪 Tests

```bash
flutter test
```

Covers the normalization engine and level entity/grid math. Add gameplay tests alongside new features (see CONTRIBUTING.md).

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). UI strings must be Arabic only; RTL must be verified on device.

## 📄 License

MIT — see [LICENSE](LICENSE).
