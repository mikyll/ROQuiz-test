# roquiz

Applicazione per esercitarsi con i quiz di Ricerca Operativa M.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

## Build

To build the application, first install Flutter. Then run the following command:

```bash
flutter build <platform>
```

Available platforms are:

- `android`
- `windows`

## Run

## Test

To run 

## TODO

- [ ] fetch new questions at startup (on web is useless probably?)
- [x] snackbar issue
- [x] in view_settings, you must be able to disable a set of settings:
  - [x] when going from view_menu, if you change the quiz number and go back, you get a range error

- [ ] fix commits

- [ ] CHANGELOG
- [ ] release tempalte





I need a big help on git refactor for timestamps... I want you to rebase this branch to a time that makes sense for the commits it contains. I also want


fix(questions): add 4 new questions
    
    Co-Authored-By: Tomassone <86415106+Tomassone@users.noreply.github.com>


1. Android release signing (prerequisite — largely user-run, one-time)                                                   │
│                                                                                                                          │
│ User runs locally (documented in plan / README, not committed):                                                          │
│ keytool -genkey -v -keystore upload-keystore.jks -keyalg RSA -keysize 2048 \                                             │
│   -validity 10000 -alias upload                                                                                          │
│ base64 -w0 upload-keystore.jks   # value for the ANDROID_KEYSTORE_BASE64 secret                                          │
│ Add repo secrets (Settings → Secrets and variables → Actions):                                                           │
│ ANDROID_KEYSTORE_BASE64, ANDROID_KEYSTORE_PASSWORD, ANDROID_KEY_ALIAS,                                                   │
│ ANDROID_KEY_PASSWORD.                                                                                                    │
│                                                                                                                          │
│ Edit roquiz/android/app/build.gradle.kts to load an optional                                                             │
│ key.properties and use a real release signingConfig, falling back to debug when                                          │
│ absent so flutter run --release still works locally:                                                                     │
│ - Read android/key.properties if it exists into a Properties object.                                                     │
│ - Add signingConfigs { create("release") { ... storeFile/storePassword/keyAlias/keyPassword from key.properties ... } }. │
│ - In buildTypes.release, use the release signing config when key.properties                                              │
│ exists, else keep signingConfigs.getByName("debug").                                                                     │
│ - Ensure roquiz/android/key.properties and *.jks/*.keystore are gitignored                                               │
│ (check/append roquiz/android/.gitignore and root .gitignore).


---

Resume prompt for tomorrow

Copy-paste this to pick up where we left off:

Resume the Release CI/CD work on branch refactor/v2. Read SESSION.md (ACTIVE TASK
section) and the plan at
/home/mirighi/.claude-personal/plans/i-d-like-to-setup-distributed-marshmallow.md first.

The plan is approved. Implement it:
1. Create .github/workflows/release.yml (workflow_dispatch + version input; jobs:
   build-android [debug-signed for now, with a commented ready-to-enable keystore
   step], build-linux [tar.gz], build-web, release [tag v<version> + GitHub Release
   with auto notes + SHA256SUMS], deploy-web).
2. Add the inert key.properties release-signing scaffold to
   roquiz/android/app/build.gradle.kts (falls back to debug when no keystore).
3. Fix the stale app-mobile/flutter_application paths in
   .github/workflows/deploy_to_gh_pages.yml -> roquiz.
4. Gitignore key.properties, *.jks, *.keystore under roquiz/android.
5. Verify locally: flutter build apk --split-per-abi / linux / web all pass and
   flutter analyze is clean; lint the workflow YAML.

Keep Android on debug signing this pass (no keystore/secrets). Follow the repo commit
convention: single-line Conventional Commits, no co-author trailer, no Claude footer,
stage only relevant files. Update SESSION.md as you go.

Two small notes for tomorrow:
- We were interrupted right at the plan-approval step, so nothing has been implemented yet — the plan is final but no workflow/gradle files are changed.
- The dry-run (0.0.0-test) can run with no secrets since we're staying on debug signing, so you can fully test the pipeline end-to-end before deciding on a real keystore.
