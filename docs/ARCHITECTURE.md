# Architecture (high level)

A simplified overview of The Second Edition. Internals are intentionally left out.

## How it fits together

```mermaid
flowchart LR
  A[Android app] --> D[(Closet stored on device)]
  W[Web / iPhone home-screen app] --> D
  A --> P[AI proxy]
  W --> P
  P --> G[Gemini AI]
  A --> S[Status and limits service]
  W --> S
  C[Owner control app] --> S
  A -.optional.-> B[Your own Google Drive backup]
  W -.optional.-> B
```

- **The closet lives on the device.** Clothes, photos, outfits, calendar, stylist notes and songs never need a server.
- **The AI proxy** sends photos and text to Gemini and keeps nothing.
- **The status and limits service** handles daily AI limits, app status checks, push notifications, feedback and crash reports.
- **The owner control app** (Android and Windows) shows which builds are in use and controls rollouts, AI limits and the kill switch.

## How it ships

```mermaid
flowchart LR
  C[Code] --> T[Unit, end-to-end and security checks]
  T --> B1[Android build]
  T --> B2[Web deploy]
  T --> O[Over-the-air updates]
  B1 --> M[Members]
  B2 --> M
  O --> M
  K[Control app] -. minimum build, kill switch .-> M
```

## Design principles

1. **Local-first.** Your closet lives on your device and works without a server for everyday use.
2. **No keys in the app.** AI calls go through a cloud proxy, so nothing sensitive ships to phones.
3. **You own your backups.** Optional backup goes to the user's own Google Drive, not mine.
4. **The server sets the limits.** AI limits come from the server, so the app can't change them.
5. **Keep as little as possible.** Photos are processed, not stored. Song links, city and stylist notes stay on the phone.
6. **One codebase, three platforms.** Android, web and the iPhone home-screen app share the same code.
7. **Ship safely.** Tests before every release, small fixes over the air, and a way to stop a bad release.

## What's intentionally not here

Source code, configuration, keys, account details, deployment details and internal mechanisms.
