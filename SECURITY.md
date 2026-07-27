# Security Policy

## Reporting a vulnerability

If you've found a security issue in Hearth (the launcher, the installer, the client plugin, the server, or the admin API), please **do not** open a public GitHub issue.

Report it privately through GitHub: **[open a security advisory](https://github.com/HumanGenome/Hearth/security/advisories/new)** (Security > Advisories > "Report a vulnerability"). The report is visible only to you and the maintainers until a fix ships.

That is the only reporting channel. There is no security mailing address; anything sent to one will not reach us.

Include:
- A description of the vulnerability
- Steps to reproduce
- Affected component (launcher / installer / client plugin / server / admin API)
- Hearth version
- Whether the issue is currently being exploited

We aim to acknowledge reports within 72 hours and provide a triage update within 7 days.

## Scope

In scope:
- Remote code execution or unauthenticated takeover of `HearthServer.exe`
- Authentication bypass on the join handshake, RCON, or the admin API
- Code execution or privilege escalation through the launcher, its installer, or its auto-update path
- IPC injection through the client plugin or the HearthServer named pipe
- Save file corruption that lets a connected client write arbitrary host files
- Privilege escalation through plugin hooks

Out of scope:
- Hardware-host vulnerabilities (those belong to your hosting provider)
- Vulnerabilities in retail Bellwright itself (report to the game's publisher)
- Vulnerabilities in third-party mods running on Hearth
- Anti-cheat / cheating concerns — Hearth does not provide anti-cheat

Server-source issues can also be reported on [HearthServer](https://github.com/HumanGenome/HearthServer/security/advisories/new); either advisory form reaches the same maintainers.
