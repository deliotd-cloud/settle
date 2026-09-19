# Settle

Independent, offline Life in the UK test preparation app for Android, published by CuraeVita.

- Package: `uk.co.settleapp.lifeintheuk`
- Version: `1.0.0` (1)
- Support: eliviontechnologies@gmail.com
- Privacy: https://deliotd-cloud.github.io/settle/privacy-policy.html

## Build

Use Node.js 20+, JDK 21 and Android SDK platform/build-tools 36.

```sh
npm ci
npm run sync
cd android
./gradlew bundleRelease
```

Create an upload key and supply `android/keystore.properties` using the example. Keep the key and passwords private and backed up. Play App Signing manages the distribution signing key.

The app uses Capacitor 7. Fonts and study content are bundled. Native text-to-speech selects installed offline English voices. Download an English voice in Android settings if required. There is no microphone permission, account, advertising or analytics SDK.

## Release validation

The release bundle builds successfully, including the release lint check. Browser checks cover the main routes, scoring, persistence and phone layout. Store screenshots use sample progress in a separate browser profile; that data is not packaged in the app. Physical-device validation remains outstanding.

## Public policy

GitHub Pages serves only `docs/`. The Android app is a paid Google Play download.
