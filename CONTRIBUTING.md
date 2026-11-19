# Contributing

Thanks for wanting to improve Windows Memory Cleaner — Next Generation. This is a
proprietary project: the source code is not published, so there is nothing to fork
or send a pull request against. What is genuinely useful is a clear report.

## Reporting a bug

Open an issue on the repository and include:

1. **What you did** — the page and the exact button or setting you used.
2. **What you expected** and **what happened instead**. Quote any error text.
3. **Your Windows version and build** — run `winver`, or read it from the status
   bar at the bottom of the application window.
4. **Whether the application was running as Administrator.** The status bar says
   `Standard user · Elevate` when it is not elevated, which changes what the
   memory operations can do.
5. **The log.** It is at:

   ```
   %APPDATA%\WindowsMemoryCleanerNG\logs\application.log
   ```

   Attach the last ~200 lines around the time of the problem. It contains no
   personal data beyond your Windows user name.

Please do not attach screenshots of the log when text will do — the text can be
searched.

## Requesting a feature

Describe the problem you are trying to solve before describing the feature. A
request that says "my machine swaps when I have two browsers open" leads somewhere;
"add a turbo mode" does not.

Some things will not be accepted, because they are unsafe or dishonest:

- Terminating processes the application did not start.
- Touching services, scheduled tasks, the pagefile, SSD/TRIM settings, firmware,
  Windows Defender, the firewall or Windows Update.
- Anything that reports memory as released without measuring it.
- Claims that the application permanently speeds up a machine. Windows refills the
  cache after any purge; that is by design.

## Reporting a security problem

Do **not** open a public issue. Follow the private route described in
[SECURITY.md](<SECURITY.md>).

## Code contributions

Not accepted. The project is proprietary and the source is not distributed, so
there is no mechanism for outside code to be reviewed, merged or licensed. This is
a licensing limitation, not a comment on anyone's work.

## Translations and documentation

Corrections to this documentation, spelling errors and clearer wording are welcome
as issues. Say which file and which line.

## Conduct

Be specific and be civil. Reports that cannot be reproduced will be closed with a
request for more detail, not dismissed — but a report with no version, no steps and
no log cannot be acted on.
