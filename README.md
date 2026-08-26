# Media Mermaid — Android App

The Emblematic (Media Mermaid) logo-builder web app packaged as a native
Android app with [Capacitor](https://capacitorjs.com/). The project is
self-contained: the Capacitor Android core library is vendored under
`capacitor-android/` (see `capacitor.settings.gradle`), so it builds with no
`node_modules` tree required.

## Building the release AAB

A GitHub Actions workflow (`.github/workflows/build-aab.yml`) builds a release
Android App Bundle on every push / pull request to the default branch, and can
also be triggered manually:

1. Open the repo on GitHub → **Actions** → **Build Android AAB**.
2. Click **Run workflow** → **Run** (or just push to `main`).
3. When the job finishes, the bundle `app-release.aab` is available as a build
   artifact: open the finished run and download it under **Artifacts**.

The raw output path inside the runner is
`app/build/outputs/bundle/release/app-release.aab`.

## Signing

Signing secrets are **optional**. The `app/build.gradle` signs the release
bundle only when the following GitHub secrets exist:

| Secret              | Purpose                                   |
|---------------------|-------------------------------------------|
| `KEYSTORE_BASE64`   | Base64-encoded release keystore (`.jks`)  |
| `KEYSTORE_PASSWORD` | Keystore password                         |
| `KEY_ALIAS`         | Key alias (defaults to `emblematic`)      |
| `KEY_PASSWORD`      | Key password (defaults to the keystore password) |

- With the secrets set, the workflow decodes `KEYSTORE_BASE64` to a temp
  `.jks` file and the build produces a **signed** AAB, ready for Play Store.
- Without them, the build still succeeds and produces an **unsigned** AAB
  (suitable for testing; Play Store requires a signed bundle).

No keystore file is committed to this repository.

## Local development

Requires JDK 21 and the Android SDK (`platforms;android-36`,
`build-tools;36.0.0`):

```sh
./gradlew :app:bundleRelease   # release AAB
./gradlew :app:assembleDebug   # debug APK
```