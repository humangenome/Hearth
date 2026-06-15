<p align="center">
  <img src="docs/img/hearth-lockup-dark.png" alt="Hearth" width="420">
</p>

# Hearth

[![Platform](https://img.shields.io/badge/Platform-Windows_10%2F11-blue.svg)](#install)
[![Game](https://img.shields.io/badge/Game-Bellwright-darkgreen.svg)](https://store.steampowered.com/app/1812450/)
[![Server Source](https://img.shields.io/badge/Server_Source-HearthServer-brightgreen.svg)](https://github.com/HumanGenome/HearthServer)

Hearth is a hosting stack for **[Bellwright](https://store.steampowered.com/app/1812450/)** (UE5.7)
multiplayer. Players join through the Hearth launcher by IP and port; hosts run
**HearthServer** next to Bellwright, and the world stays on the server with crash
recovery, scheduled restarts, admin tools, server query, and RCON.

This is the **launcher hub** — download the installer here. The dedicated-server
source lives in [HumanGenome/HearthServer](https://github.com/HumanGenome/HearthServer).

## Install

Download and run the latest installer:

**[⬇ HearthSetup-latest.exe](https://github.com/HumanGenome/Hearth/releases/latest/download/HearthSetup-latest.exe)**

The launcher keeps itself up to date automatically (signed update manifest) — you
only install once. Windows 10/11, no admin rights required (installs per-user).

## How it works

- **Launcher** — add a server by IP/port, save credentials, launch Bellwright
  straight into it, and watch live status (online/offline via A2S query).
- **HearthServer** — the dedicated-server supervisor that runs next to Bellwright
  on the host: process supervision, crash recovery, A2S query, Source RCON, and
  SQLite-backed bans/schedule/audit. Source: [HumanGenome/HearthServer](https://github.com/HumanGenome/HearthServer).

## Self-hosting

HearthServer is open source and fully self-hostable — build it from
[its repository](https://github.com/HumanGenome/HearthServer) and point the Hearth
launcher at your machine's IP and port.

## Official hosting

Prefer managed? [SurvivalServers.com](https://www.survivalservers.com/games/bellwright/)
runs Hearth pre-installed and kept on the latest pinned release.

## License

[MIT](LICENSE) © HumanGenome
