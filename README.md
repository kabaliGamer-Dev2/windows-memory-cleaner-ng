# Windows Memory Cleaner — Next Generation

> **[Download the latest build](https://github.com/kabaliGamer-Dev2/windows-memory-cleaner-ng/releases/latest)** · **[Visit the website](https://kabaligamer-dev2.github.io/windows-memory-cleaner-ng-website/)** — screenshots, feature tour and FAQ.

A Windows desktop app for running the system's own memory-management operations on demand, and seeing exactly what happened.

Created and developed by **Kabbali Gamer**.

## What this is

Windows Memory Cleaner — Next Generation is a native Windows application built with Tauri 2, a React 19 + TypeScript interface, and a Rust backend that calls the real Windows memory APIs. It lets you release process working sets, clear the standby list, flush the modified page list and the file cache, and it records every run in a history you can read afterwards. It also shows what Windows launches at sign-in and lets you switch those programs off. There is no Electron shell and no .NET runtime, and the app makes no network connections at runtime.

## Before you expect magic

Windows already manages memory well. It caches aggressively on purpose: those cached pages are why launching a program the second time feels fast, and memory sitting in the standby list is memory any application can claim the moment it asks for it.

Clearing memory frees RAM now, but Windows refills it — that is what it is designed to do. It is not a permanent speed-up, and for a few minutes after a full purge the machine can feel slower, because data that had been cached has to be read from disk again.

The change that durably reduces what Windows loads at sign-in is fewer startup programs. That is why this app has a **Startup Apps** page, and it is the page most people should look at first.

This app does not promise performance gains. It gives you deliberate access to specific documented controls, and it tells you honestly when an operation released almost nothing.

## Install

### Installer (recommended)

1. Download [`WindowsMemoryCleanerNG_2.0.0_x64-setup.exe`](https://github.com/kabaliGamer-Dev2/windows-memory-cleaner-ng/releases/latest/download/v2.0.0/WindowsMemoryCleanerNG_2.0.0_x64-setup.exe).
2. Run it. It installs for the current user and does not need administrator rights to install.
3. Launch **Windows Memory Cleaner NG** from the Start Menu or the desktop shortcut.

The installer:

- installs to `%LOCALAPPDATA%\Windows Memory Cleaner NG`
- creates Start Menu and desktop shortcuts
- registers the app in **Settings > Apps** as "Windows Memory Cleaner NG"

For a silent install:

```powershell
WindowsMemoryCleanerNG_2.0.0_x64-setup.exe /S
```

### Portable

`WindowsMemoryCleanerNG.exe` runs with no installation. Put it anywhere you like and launch it. It stores its settings and history in the same location as the installed version, described in [Where your settings live](#where-your-settings-live).

### Requirements

- Windows 10 version 1809 or later, or Windows 11, x64
- The Microsoft Edge WebView2 runtime. Windows 11 and current Windows 10 builds already include it; the installer ships a bootstrapper for machines that do not.

## How to use it

1. **Open the app.** The window opens on the Dashboard.
2. **Read the Dashboard.** It shows current memory figures and whether you are running elevated.
3. **Go to Memory Cleaner and pick operations.** Each row shows what the operation does and whether your current permission level allows it.
4. **Click _Run selected_.** If the system-wide operations are not available, the app offers **Run as Administrator** instead of failing quietly.
5. **Check Activity.** Every run is recorded there with its trigger, how long it took, and the result of each operation.
6. **Set up Automation only if you have a pattern worth automating.** It is off by default; see below.

Monitoring, history, the Startup Apps page and the app's own working-set trim all work without administrator rights. The system-wide operations need them, and the app asks rather than silently doing nothing.

## What each page does

| Page | What it does |
| --- | --- |
| **Dashboard** | Current memory load, available, cached and committed, plus your elevation state. |
| **Memory Cleaner** | The individual cleanup operations. Pick them, run them, read the result. |
| **Memory Monitor** | A live chart of load, available, cached and committed over time. |
| **Processes** | Running processes with memory figures, and a way to trim one process's working set. |
| **Automation** | Optional scheduled or threshold-triggered cleaning. |
| **Activity** | History of every cleanup, with trigger, duration and per-operation result. |
| **Startup Apps** | Programs Windows launches at sign-in, and per-user switches to turn them off. |
| **Settings** | Theme, density, motion, monitoring interval, history depth, exclusions, shortcuts and more. |

### The Memory Cleaner operations

- **Working Sets** — asks processes to release memory they loaded but are not using.
- **Standby List, low priority** — discards only the coldest cached pages.
- **Standby List** — full purge. This one is marked disruptive in the interface, for good reason.
- **Modified Page List** — writes dirty pages to disk, then moves them to standby.
- **System File Cache** — flushes the operating system's file cache.
- **Combined Page List** — flushes the combined page list.
- **Registry Cache** — reconciles cached registry hive data.
- **Volume Cache** — flushes file system write buffers.

Every row shows whether the current permission level allows it. Operations that need elevation are marked, not hidden.

### The Processes page

It lists running processes with their memory figures and can trim an individual process's working set. It never terminates a process.

## Automation

Automation is opt-in. When it is on, you choose how a run is triggered:

- **Memory threshold** — presets of 50, 60, 70, 75, 80, 85 and 90 percent, plus a custom value.
- **Fixed interval** — presets of 15 m, 30 m, 1 h, 2 h, 4 h, 6 h, 12 h and 24 h, plus a custom value.
- **Either** — runs when the threshold is crossed, or when the interval elapses.

Two guards keep it from being noisy. A **cooldown** separates runs, and a **threshold hysteresis** band stops memory load that hovers at your limit from firing repeatedly. A run is also skipped when the selected operations have less than 64 MiB to release, so a short interval never turns into a string of pointless purges.

You can **pause and resume** automation at any time, including from a keyboard shortcut. The page shows the next run, the last run, and the reason the last run was skipped.

Automatic cleaning requires Administrator rights. The app detects that, and offers **Run as Administrator** rather than failing silently. If you want it elevated from the start, a setting makes it ask for administrator rights at launch.

## Startup Apps — the part that actually lasts

This is the page that changes something permanent. It lists the programs Windows launches when you sign in, collected from the `HKCU` and `HKLM` Run keys and from both Startup folders.

Per-user entries get a switch you can turn off and turn back on at any time, reversibly. Machine-wide entries are listed read-only, because switching those off would affect every account on the PC. Nothing is uninstalled, nothing is deleted, and no files are moved.

Disabling a startup entry does not make Windows faster in a burst. It reduces what Windows loads at every sign-in, which is the only memory saving that persists.

## Keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| `Ctrl+Alt+M` | Optimise now |
| `Ctrl+Alt+P` | Pause or resume automation |
| `Ctrl+Alt+S` | Open the window |

All three are configurable in Settings, and they can be turned off entirely if they collide with something else.

## Safety and what it will not do

This app changes only what you ask it to change, and nothing else. In particular it:

- never terminates a process
- never touches services, scheduled tasks or the pagefile
- never changes SSD or TRIM settings, or firmware
- never touches Windows Defender, the firewall or Windows Update
- never edits the registry beyond its own optional start-with-Windows value and the StartupApproved flags described above
- makes no network connections at runtime, and collects no telemetry

The Processes page trims a working set; it does not kill anything. The Startup Apps page toggles an entry; it does not remove the program.

## Where your settings live

```text
%APPDATA%\WindowsMemoryCleanerNG\settings.json
%APPDATA%\WindowsMemoryCleanerNG\history.json
%APPDATA%\WindowsMemoryCleanerNG\logs\application.log
```

`settings.json` holds your preferences, including process exclusions. `history.json` holds the cleaning history, trimmed to the depth you choose. `logs\application.log` is written at the log level you select in Settings, which is the first place to look if something behaves unexpectedly.

## Uninstall

1. Open **Settings > Apps**, find **Windows Memory Cleaner NG**, and choose Uninstall. Add/Remove passes the request through to Windows.
2. Alternatively, run `uninstall.exe` from the install folder at `%LOCALAPPDATA%\Windows Memory Cleaner NG`.

The app's data folder is separate from its install folder. If you want the settings and history gone too, delete `%APPDATA%\WindowsMemoryCleanerNG\` as well.

## Build from source

You need Rust stable with the `x86_64-pc-windows-msvc` target, Node 20 or newer, pnpm, and the MSVC build tools with the Windows SDK.

```powershell
pnpm install
pnpm tauri build
pnpm typecheck
node scripts/verify-app.mjs

cd src-tauri
cargo test
```

The repository ships 127 automated tests, builds clippy clean, and includes a 29-check acceptance harness (`node scripts/verify-app.mjs`) that runs against the real built binary.

`CONTRIBUTING.md` covers development setup and pull request expectations. `docs/` holds the architecture, build and design documentation.

## License

Proprietary. Copyright © 2026 Kabbali Gamer. All rights reserved.

This repository publishes information about the application, not its source
code. The compiled application is distributed as an installer and a portable
executable. You may use it and pass the unmodified installer on, but you may
not modify, reverse engineer, repackage or redistribute it as your own work.
Full terms are in [LICENSE](<LICENSE>).

The bundled DM Sans typeface is licensed separately under the SIL Open Font
License 1.1 — see [docs/CREDITS.md](<docs/CREDITS.md>).