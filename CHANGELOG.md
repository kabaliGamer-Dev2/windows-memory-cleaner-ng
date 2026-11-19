# Changelog

All notable changes to Windows Memory Cleaner - Next Generation are documented in
this file.

The format is based on Keep a Changelog, and this project follows Semantic
Versioning.

## [Unreleased]

No unreleased changes.

## [2.0.0]

Release date not yet recorded.

### Added

- Dashboard UI: a dashboard route plus seven working pages - Memory Cleaner,
  Memory Monitor, Processes, Automation, Activity, Startup Apps and Settings.
- Memory cleanup operations: working-set trimming, standby list purge, modified
  page list flush, system file cache flush, combined page list, and registry
  cache reconciliation.
- Live memory monitor with charts.
- Process list with per-process working-set trimming.
- Automation with interval and threshold triggers.
- Startup-apps management, with reversible enable and disable through the Windows
  `StartupApproved` mechanism.
- Activity history.
- Settings persistence across restarts.
- Single-instance guard, so only one instance of the application runs at a time.
- One-click "Clean as Administrator" elevation.
- A 29-check post-build acceptance harness (`scripts/verify-app.mjs`).

### Security

- Webview permissions reduced to two: `core:event:allow-listen` and
  `core:event:allow-unlisten`.
- CSP `unsafe-inline` removed, so the shipped policy permits local assets only.
- `cargo audit` and `pnpm audit` report no known vulnerabilities.
