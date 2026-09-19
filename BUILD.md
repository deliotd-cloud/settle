# Settle — build and release guide

A Capacitor wrapper around a single self-contained HTML app. No server, no network
calls, no accounts. Everything ships inside the APK.

---

## 1. Prerequisites

* **Node.js 20+** and npm
* **Android Studio** (Ladybug or newer) with the Android 16 / API 36 SDK installed
* **JDK 21** (bundled with recent Android Studio)

## 2. First build

```bash
npm install            # restores Capacitor — required, node_modules is not shipped
npx cap sync android   # copies www/ into the native project
npx cap open android   # opens Android Studio
```

In Android Studio, let Gradle sync, then **Run** on a device or emulator.

Whenever you edit `www/index.html`, run `npx cap sync android` again.

## 3. Signing

Play requires an upload key. Create one once and keep it safe — losing it means you
can never update the app.

```bash
keytool -genkey -v -keystore settle-upload.jks -keyalg RSA \
        -keysize 2048 -validity 10000 -alias settle
```

Store the `.jks` **outside** the project, then create `android/keystore.properties`:

```properties
storeFile=/absolute/path/to/settle-upload.jks
storePassword=YOUR_STORE_PASSWORD
keyAlias=settle
keyPassword=YOUR_KEY_PASSWORD
```

`app/build.gradle` already reads this file and applies the release signing config.
The file is git-ignored. Also enrol in **Play App Signing** when you first upload —
Google then holds the app signing key and your upload key becomes replaceable.

## 4. Release bundle

```bash
cd android
./gradlew bundleRelease
```

Output: `android/app/build/outputs/bundle/release/app-release.aab` — upload this.

## 5. Version bumps

Edit `android/app/build.gradle`:

* `versionCode` — integer, must increase on every upload (1, 2, 3 …)
* `versionName` — what users see ("1.0.0", "1.1.0" …)

---

## What is already configured

| Item | Value | Why |
|---|---|---|
| Application ID | `uk.co.settleapp.lifeintheuk` | Permanent once published — change it now if you want a different one |
| targetSdk / compileSdk | **36** | Google Play requires API 36 for all new apps and updates since 31 Aug 2026 |
| minSdk | 23 (Android 6) | Covers ~99% of active devices |
| Launcher icon | Adaptive, navy field + flag roundel | Includes a monochrome layer for Android 13+ themed icons |
| Launch screen | `windowSplashScreenBackground` `#06122B` + roundel | Uses the Android 12+ splash API, so it flows into the in-app animation |
| Permissions | `INTERNET` only | Capacitor's local asset loader uses it. The app makes no network requests — verify with Android Studio's Network Inspector, and you may remove the line from `AndroidManifest.xml` if it still launches cleanly on your test devices |
| Orientation | Unlocked | API 36 forbids orientation locking on screens 600dp+; the layout is responsive and centred |

## Edge-to-edge

Android 16 removes the edge-to-edge opt-out. The app already declares
`viewport-fit=cover` and pads with `env(safe-area-inset-*)`, so content stays clear
of the status and gesture bars. Test once on a device with gesture navigation.

## Fonts

See `www/fonts/README.txt`. The app falls back to platform fonts if you ship
without them; drop two woff2 files in to get the exact design. Either way there is
no network request.
