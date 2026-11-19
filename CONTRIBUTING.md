# Contributing

Contributions to Windows Memory Cleaner - Next Generation are welcome. It is a
Tauri 2 application with a React 19 and TypeScript frontend and a Rust backend.
Keep changes small, explain the reason, and leave the tree passing the checks below.

Note: the project licence is still unresolved (see `LICENSE-DECISION.md`), so the
terms under which a contribution is accepted depend on the owner's choice.

## Prerequisites

- Windows 10 1809 or newer, or Windows 11, x64.
- Rust stable for the `x86_64-pc-windows-msvc` target. `Cargo.toml` declares a
  minimum supported Rust version of 1.77.
- Node.js 20 or newer, and pnpm.
- Visual Studio Build Tools with the "Desktop development with C++" workload,
  including the Windows SDK and the MSVC toolchain.
- The WebView2 runtime (Windows 11 includes it; the installer can download it on older builds).
- The Tauri CLI is used through pnpm, so no global install is needed.

## Build and run

```
pnpm install
pnpm tauri:dev      # development app; Vite serves the frontend on port 1420
pnpm build          # typecheck plus the production frontend bundle
pnpm tauri:build    # release build and NSIS installer
```

The installer is written under `src-tauri/target/release/bundle/nsis/`. Rust
commands run from `src-tauri/`, because that is where the Cargo workspace lives.

## Tests and checks

Run these before opening a pull request:

```
pnpm typecheck
cargo test --manifest-path src-tauri/Cargo.toml
cargo clippy --manifest-path src-tauri/Cargo.toml --all-targets -- -D warnings
cargo fmt --manifest-path src-tauri/Cargo.toml --check
```

The post-build acceptance harness drives the real release binary over the Chrome
DevTools Protocol and asserts the properties that matter for a shipping build -
pages mount, memory figures are self-consistent, no console error or CSP
violation, no horizontal overflow at the minimum window size, and an unelevated
cleanup fails honestly instead of claiming success. Run it after a release build:

```
pnpm tauri:build
node scripts/verify-app.mjs
```

It uses 29 checks and exits non-zero on any failure. To point it at an existing
binary or another debugging port:

```
node scripts/verify-app.mjs --exe path\to\win-memory-cleaner-ng.exe --port 9231
```

The tree is expected to pass every command above. No CI workflow is configured
yet, so these gates are enforced in review; a pull request that fails one is not
merged.

## Coding rules

1. **`unsafe` stays in `src-tauri/src/winapi/`.** That directory is the FFI
   boundary; no other module may call an unsafe function or touch a raw pointer.
   Where you add an unsafe block, state the invariant in a `// SAFETY:` comment.
2. **No new dependency without justification.** Explain what it replaces, why
   the standard library or an existing dependency is insufficient, its licence,
   and the audit result. Keep the `windows-sys` feature list limited to the APIs
   actually used.
3. **CSS uses the design tokens.** Colours, spacing, radii and type values come
   from `src/styles/tokens.css`. Do not hard-code new colours in components.
4. **Clippy and rustfmt are clean.** `cargo clippy --all-targets -- -D warnings`
   and `cargo fmt --check` must exit 0.
5. **Do not widen the webview surface.** The capability set is two event
   permissions (`core:event:allow-listen`, `core:event:allow-unlisten`) and the
   CSP allows only local assets. A change that needs a new permission or a
   relaxed CSP needs an explicit security argument in the pull request.
6. **Do not overstate behaviour in UI copy.** The application never terminates
   processes and never touches services, the pagefile, firmware, Defender, the
   firewall or Windows Update. Wording must match what the code does.
7. **New Windows API calls need a reason.** Say which documented API is used,
   what it changes about the system, and what happens when the call fails or is
   denied.

## Proposing a change

- Open an issue first for a bug (reproduction, expected and actual result) or for
  a feature (short proposal and the user problem it solves).
- Keep pull requests small and focused on one concern. Do not mix a refactor with
  a behaviour change, and do not reformat untouched files.
- Describe what changed and why, the risk, and the test evidence - the commands
  you ran and their result. Include a screenshot for a UI change.
- Do not commit build output, `node_modules/`, `src-tauri/target/` or installers.

## Commit messages

Use the imperative mood in the subject, keep it under about 72 characters, and do
not end it with a full stop. The body explains the why, not a restatement of the
diff.

```
Honour the existing StartupApproved value when toggling

StartupApproved stores a binary blob, and writing a fresh value re-enabled
entries the user had disabled. Read the current value first and flip only the
bit that controls the entry.
```

## Contribution licence

The project is proprietary: copyright © 2026 Kabbali Gamer, all rights reserved. See LICENSE. Contributions are accepted only with a signed statement that you transfer copyright to the author, because the project cannot accept code under a licence it does not hold.
See `LICENSE-DECISION.md`. Contributions are accepted only under the licence in
effect when they are merged. If the owner selects the proprietary option, a
contributor licence agreement may be required before a change can be merged.
