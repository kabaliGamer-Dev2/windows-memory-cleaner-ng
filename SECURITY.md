# Security policy

This document describes the security posture of Windows Memory Cleaner - Next Generation and how to report a problem in it.

## Supported versions

| Version | Supported |
|---|---|
| 2.0.x | Yes |
| Earlier than 2.0 | No - no earlier public release exists, and there is no LTS branch |

## Reporting a vulnerability

Report privately. Do not open a public issue, and do not post details publicly
before a fix or an agreed disclosure date.

The intended channel is GitHub private vulnerability reporting on the project
repository (Security > Report a vulnerability). **The repository has not been
confirmed by the owner yet**, so this document cannot name it; see
`LICENSE-DECISION.md`. Until then, do not disclose a suspected vulnerability publicly.

A useful report contains:

- the application version, from the window title or the installer;
- the Windows version and build (`winver`), and whether it ran elevated;
- exact steps, expected and actual result, with any destructive PoC steps removed;
- relevant lines from `%APPDATA%\WindowsMemoryCleanerNG\logs\application.log`.

Please do not send memory dumps or full process listings. The owner will
acknowledge the report and agree a disclosure date before details are published;
no fixed response-time SLA is published.

## What the application does and does not do with privileges

The application runs as the logged-in user. Memory figures are read through
documented Windows APIs (primarily `GlobalMemoryStatusEx`). Several cleanup
operations need administrative rights, so **"Clean as Administrator"** asks for
elevation through the normal UAC prompt; the elevated process performs
memory-management calls only and gains no other capability.

The operations it may perform, when the user asks for them:

- trim the working set of processes (`K32EmptyWorkingSet`);
- purge the standby list, flush the modified page list and flush the system file
  cache (`NtSetSystemInformation`, `SetSystemFileCacheSize`);
- reconcile the registry cache;
- trim the working set of a single process the user selects in the process list -
  this changes memory residency, it does not end the process;
- enable or disable startup entries the user selects, using the reversible
  `StartupApproved` mechanism.

It does **not**:

- terminate, suspend or inject into arbitrary processes;
- start, stop or reconfigure Windows services;
- touch the pagefile, SSD/TRIM state, firmware, drivers, Windows Defender, the
  firewall or Windows Update;
- write outside its own `%APPDATA%` folder and the startup entries the user
  explicitly toggles;
- install a service, driver, scheduled task or background updater.

## Threat model in brief

The runtime attack surface is small by construction:

- the webview can call only this application's own commands, plus the two event
  permissions `core:event:allow-listen` and `core:event:allow-unlisten`; no
  filesystem, shell, HTTP, window, tray or notification permission is granted;
- the shipped CSP allows only `'self'` for scripts, styles and fonts, with no
  `unsafe-inline`, and `freezePrototype` is enabled;
- there is no network listener and no outbound connection at runtime;
- a second instance is refused by a per-session mutex, so only one process owns
  the settings, history and log files;
- settings, history and log files live under the user's `%APPDATA%`, so their
  integrity is that of the user's own profile.

The risks that remain are the usual ones for a local desktop tool: a compromised
webview (mitigated by the minimal permission set and the CSP, not eliminated), a
same-user process tampering with the settings or log files, the UAC boundary
around the elevated path, and supply-chain risk in the installer and dependencies.
Physical access, an already-administrator attacker and other processes' memory are
out of scope.

## Data stored, and where

| Path (under `%APPDATA%\WindowsMemoryCleanerNG\`) | Contents |
|---|---|
| `settings.json` | user preferences: theme, automation interval and threshold, display options |
| `history.json` | cleanup and activity history shown on the Activity page |
| `logs\application.log` | rolling application log; may contain process names and file paths |

No telemetry, analytics, crash reporting or usage data is collected or
transmitted. To remove all stored data, close the application and delete
`%APPDATA%\WindowsMemoryCleanerNG`; it is recreated with defaults on the next start.

## Network

The application makes **no network connections at runtime**. Fonts, styles and
scripts are bundled; the webview loads local assets only.

One exception belongs to installation rather than runtime: the installer is
configured with Tauri's WebView2 `downloadBootstrapper` mode, so if the Microsoft
WebView2 runtime is missing, the installer may download it once, from Microsoft.

## Before publishing

This policy becomes final only after the owner confirms the repository location
and the licence, as recorded in `LICENSE-DECISION.md`; the reporting channel above
must then be replaced with the confirmed repository's private reporting link.
