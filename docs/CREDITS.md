# Credits

Windows Memory Cleaner - Next Generation.

## Created by

**Created and developed by Kabbali Gamer.**

Copyright © Kabbali Gamer. All rights in the application, its source code, its
design and its documentation belong to the author, subject to the third-party
notices below and the licence chosen in `LICENSE-DECISION.md`.

## Functional inspiration

The idea of exposing Windows memory-management mechanisms in a small desktop
utility is inspired by **WinMemoryCleaner** by **Igor Mundstein**:

- <https://github.com/IgorMundstein/WinMemoryCleaner>

That project is credited as functional inspiration. No source code, branding,
documentation or UI was copied from it. It is a C#/.NET WPF application; this
project is a separate implementation in Rust and TypeScript, built on the
memory-management APIs that Microsoft documents publicly. The upstream project's
name, logo and visual identity are not used here, and this project is not
affiliated with or endorsed by its author.

## Bundled font

**DM Sans**, bundled in `public/fonts/` as WOFF2 subsets.

- Copyright 2014-2024 The DM Sans Project Authors
  (<https://github.com/googlefonts/dm-fonts>)
- Licensed under the **SIL Open Font License, Version 1.1**
  (<https://openfontlicense.org/open-font-license-official-text/>)

The font is distributed as a separate work. It is bundled rather than fetched at
runtime so the application never makes a network request. The full notice,
including the licence reference and the fallback behaviour, is at
`public/fonts/OFL-NOTICE.md` and must be kept in any redistribution.

## Third-party software

This application is built with open-source software. The exact set and versions
are in `src-tauri/Cargo.toml` and `package.json`; each component remains under its
own licence, and its copyright notices apply.

**Rust crates:** the Tauri framework and its build tooling, serialization
(`serde`, `serde_json`), synchronisation (`parking_lot`), error handling
(`thiserror`), logging (`log`) and the Windows API bindings (`windows-sys`).
Generated lockfile data records every transitive dependency.

**npm packages:** React and React DOM for the interface, Zustand for state,
Framer Motion for transitions, Lucide for icons, the Tauri JavaScript API, and the
Vite and TypeScript toolchain used to build the frontend bundle.

The Windows APIs used by this application are called as documented by Microsoft;
no Microsoft source code or assets are bundled. Windows is a trademark of
Microsoft Corporation.

## Corrections

If an attribution here is missing or wrong, or if you believe a work has been
used without the credit it deserves, raise it with the owner so it can be fixed.
Corrections to this file are welcome as a contribution; see `CONTRIBUTING.md`.
