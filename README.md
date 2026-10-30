# My Planner

An offline-first productivity suite I built for my own daily use: a native **Android app** and a native **macOS desktop-widget app** that share one data format. Plan tasks, set week/month/year goals, track habits, capture notes, and stay focused, with no accounts, no servers and no tracking.

> **Status:** personal project, in daily use. Not published to any app store.

<!-- Add screenshots here -->
<!--
<p align="center">
  <img src="docs/screenshots/android-today.png" width="260" />
  <img src="docs/screenshots/android-goals.png" width="260" />
  <img src="docs/screenshots/mac-widget.png" width="260" />
</p>
-->

## Why I built it

Most to-do apps are either too simple (a flat list) or too heavy (accounts, subscriptions, sync). I wanted one place for **daily tasks, deadlines, long-term targets and habits** that lives on my own devices and keeps my data private. Building it was also a way to learn native Android and macOS development end to end, from UI to storage to CI.

## Features

| Area | What it does |
|---|---|
| **Today** | Daily progress, quick-add bar, overdue tasks highlighted, today's habits |
| **Tasks** | Priority levels, details, deadlines (date picker), All / Pending / Done filters |
| **Goals** | Week, Month and Year targets with a numeric target (e.g. "12 books"), live progress |
| **Habits** | One-tap daily check-in, streak counter, 7-day history |
| **Notes** | Free-form notes with pinning |
| **Focus timer** | 15 / 25 / 45 minute sessions |
| **Weekly review** *(Android)* | Tasks completed per day, overdue count, habit consistency, goal progress |
| **Reminders** *(Android)* | Notification at 9:00 AM on a deadline day; rescheduled after reboot |
| **Home-screen widget** *(Android)* | Today's top tasks at a glance |
| **Backup / Restore** | Plain JSON file, interchangeable between the Android and Mac apps |

## Two apps, one data format

### Android (`/Planner`)
- **Kotlin + Jetpack Compose (Material 3)**, no third-party libraries
- Custom design system: dark/light themes, dot-matrix digits rendered with Compose `Canvas`, dot-based progress bars
- Local persistence in app-private storage (JSON)
- **AlarmManager + BroadcastReceiver** for deadline notifications, plus a boot receiver to re-arm alarms
- **AppWidgetProvider / RemoteViews** home-screen widget
- Backup/restore through the **Storage Access Framework**
- Adaptive launcher icon (vector, with themed-icon support)

### macOS (`/PlannerDesk`)
- **SwiftUI + AppKit**, Swift Package Manager (no Xcode project needed)
- Borderless, translucent panel pinned at **desktop window level**, so it behaves like a widget
- Frosted glass (`NSVisualEffectView`), per-tab colour themes, animated progress ring
- Menu-bar item (show/hide, keep above windows, launch at login via `SMAppService`, backup/restore)
- Universal binary (Apple Silicon + Intel)

## Engineering highlights

- **CI/CD with GitHub Actions:** every push builds a ready-to-install APK (Android) and a universal `.app` (macOS) on GitHub's runners, so I can build both without a local toolchain.
- **Privacy by design:** all data stays on the device. The only network-free storage format is a human-readable JSON file.
- **Cross-platform data compatibility:** the same JSON schema is read and written by Kotlin (`org.json`) and Swift (`Codable`), with defaults for missing fields so older backups keep working.
- **Native UI on both platforms** rather than a web wrapper.

## Project structure

```
Planner/                       Android app
  app/src/main/java/.../
    MainActivity.kt            UI, state, storage, dialogs
    Extras.kt                  Reminders, boot receiver, home-screen widget
  app/src/main/res/            Icon, widget layout
  .github/workflows/build.yml  APK build

PlannerDesk/                   macOS app
  Sources/MyPlanner/App.swift  Model, desktop window, SwiftUI views
  Info.plist
  .github/workflows/build.yml  .app build
```

## Build it yourself

**Android:** open the project in Android Studio and run, or push to GitHub and download the APK from the *Actions* tab artifacts.

**macOS:** `swift build -c release`, or push to GitHub and download the `.app` from the *Actions* tab artifacts. The app is ad-hoc signed, so on first launch right-click it and choose **Open**.

## What I learned

- Structuring a Compose app around a single state source and persisting it safely
- Android background work: alarms, receivers, widgets, permissions (including Android 13+ notifications)
- Making a macOS app behave like a desktop widget (window levels, activation policy, status items)
- Setting up reproducible builds with GitHub Actions for two platforms

## Roadmap

- [ ] Optional cloud sync with sign-in
- [ ] Reminders on macOS
- [ ] Export weekly review as a shareable image
- [ ] Unit tests for date and streak logic

## Author

Built by **[Md Shamoel Farooqui]**. [LinkedIn](https://www.linkedin.com/in/md-shamoel/) · [Email](farooquimd005@gmail.com)
