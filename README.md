# ETHBTC

An Android WebView wrapper for [Aggr.trade](https://aggr.trade), designed to provide a dedicated, immersive landscape interface for monitoring crypto market activity on Android.

The app loads the live Aggr.trade website inside a native Android shell. It keeps WebView's built-in browser cache and web storage enabled, so site resources and client-side state can be reused between visits where the website and WebView permit it.

> **Project status:** Archived / reference project. This repository documents a lightweight Android WebView wrapper, not a fork or self-hosted copy of Aggr.trade.

## What it does

- Loads `https://aggr.trade` in an Android WebView.
- Uses a sensor-landscape activity and immersive full-screen system UI.
- Enables JavaScript, DOM storage, and Web SQL/database storage for website compatibility.
- Uses the WebView default cache policy (`LOAD_DEFAULT`).
- Enables automatic image loading, viewport scaling, and built-in pinch zoom.
- Supports website file-upload flows through Android's file chooser, including multiple selected files.
- Preserves and restores WebView navigation state across activity recreation.
- Handles the Android back action by navigating backward in WebView history before exiting.

## Caching and offline behavior

This app does **not** implement a separate application-level cache, service worker, proxy, or custom cache manager.

Caching is delegated to Android System WebView:

- `WebSettings.LOAD_DEFAULT` allows normal WebView caching behavior subject to HTTP cache headers and network state.
- DOM storage and database storage are enabled so the website can persist supported client-side data.
- The app does not guarantee offline access. Aggr.trade is a live web application and may require network connectivity, APIs, and remote services even when some static resources are cached.
- There is no custom cache-clear, cache-size, or cache-expiration control in the current code.

## Implementation

| Area | Current implementation |
|---|---|
| Platform | Native Android application |
| Language | Kotlin |
| UI | Single full-screen WebView in a FrameLayout |
| Application ID | `aggr.kryo` |
| Namespace | `aggr.kryo` |
| Version | `1.0` (versionCode `1`) |
| Minimum Android SDK | 31 (Android 12) |
| Compile / target SDK | 37 |
| Java source compatibility | 11 |
| Build system | Gradle Kotlin DSL |
| Android Gradle Plugin | 9.4.1 |

### Main components

- `app/src/main/java/aggr/kryo/MainActivity.kt` — WebView setup, navigation, file chooser integration, state save/restore, and immersive UI.
- `app/src/main/AndroidManifest.xml` — Internet permission, launcher activity, and sensor-landscape orientation.
- `app/src/main/res/layout/activity_main.xml` — Full-screen WebView layout.
- `app/src/main/res/values/` — App label, colors, and theme.
- `config/` — Exported Aggr.trade workspace/layout JSON files.

## Configuration snapshots

The `config/` directory contains exported Aggr.trade workspace configurations. These are data snapshots for reference/import into the website; the Android wrapper does not currently parse, modify, or automatically load them.

- `2x2-SMT-BTC-ETH.json`
- `5m-cvd-main_5m-cvd-main.json`
- `BTC1D_BTC1H_ETH1H_SP.json`
- `ETH1H_5M.json`
- `Last.json`
- `fixed_aggr.json`

## Build

### Requirements

- Android Studio with support for Android Gradle Plugin 9.4.1.
- Android SDK Platform 37.
- A compatible JDK / Gradle runtime as required by the installed Android Gradle Plugin.

### Build a debug APK

On macOS / Linux:

```bash
./gradlew assembleDebug
```

On Windows:

```bat
gradlew.bat assembleDebug
```

The debug APK is normally generated at:

```
app/build/outputs/apk/debug/app-debug.apk
```

Open the project in Android Studio to sync Gradle, select a connected device or emulator, and run the `app` configuration.

## Important implementation notes

- All HTTP and HTTPS navigation is kept inside the WebView; the current wrapper does not route external links to a separate browser.
- Mixed content is currently allowed with `MIXED_CONTENT_ALWAYS_ALLOW`. This is permissive and should be reviewed before using this code as a production template for other websites.
- JavaScript is enabled because Aggr.trade requires a JavaScript-capable browser environment.
- The app requests only the Internet permission in its manifest. File selection is delegated to Android's document/file picker.
- The repository contains no custom backend or local copy of Aggr.trade.

## Disclaimer

Aggr.trade and its website content are external services. This project is an independent Android wrapper and is not affiliated with or endorsed by Aggr.trade. Availability, functionality, data, and website behavior depend on the upstream service and Android System WebView.

