<p align="center">
  <img src="docs/img/hearth-lockup-dark.png" alt="Hearth" width="460">
</p>

# Hearth

[![Platform](https://img.shields.io/badge/Platform-Windows_10%2F11-blue.svg)](#install)
[![Game](https://img.shields.io/badge/Game-Bellwright-darkgreen.svg)](https://store.steampowered.com/app/1812450/)
[![Server Source](https://img.shields.io/badge/Server_Source-HearthServer-brightgreen.svg)](https://github.com/HumanGenome/HearthServer)

Hearth gives **Bellwright** IP/port dedicated-server multiplayer: players join through the Hearth launcher, hosts run HearthServer next to Bellwright, and the world stays on the server with snapshots, rollback, admin tools, server query, and scheduled restarts. No Steam invite, no host-must-be-online, no peer-to-peer session.

Every player must install Hearth to join a Hearth server. Stock Bellwright cannot connect to Hearth servers directly.

The server side is open source. The complete host package on the [HearthServer release page](https://github.com/HumanGenome/HearthServer/releases/latest) runs a joinable Bellwright server on any Windows box with a copy of the game; see [Self-hosted servers](#self-hosted-servers).

<p align="center">
  <img src="https://raw.githubusercontent.com/HumanGenome/Hearth/main/docs/img/launcher.png" alt="Hearth launcher showing a Bellwright server" width="860">
</p>

## Features

### 🧭 Join by IP and port
Add a server address once, pick your character, and connect straight into the hosted world from the launcher.

### 🛠 Admin console
HearthServer exposes Source RCON (default TCP 7780) with built-ins `help`, `status`, `players`, `ping`, `save snapshot`, `save list`, `save restore <id>`, `say`, `announce`, and `motd`:

```
> announce Server restarts in 10 minutes
chat ok (admin): Server restarts in 10 minutes
```

`say` and `announce` broadcast to the server's chat channel; `motd` sets the message of the day.

### 📡 Server query
HearthServer answers Source A2S query (default UDP 7779), so server browsers, monitoring tools, and bots can read live status, player counts, and the player list:

```
Server name : My Bellwright Server
Map         : Karvenia
Game        : Bellwright
Players     : 2 / 4
Version     : hearth-0.1.84
```

### 👤 Characters
Your character is bound to your Steam account, not to the name you type — the name box is a display name. Moving a world between servers keeps everything the world owns; personal characters stay with the Steam accounts that made them. See [docs/WORLDS.md](docs/WORLDS.md).

### 🔁 Snapshots and rollback
The server snapshots the world on auto-save, and admins can trigger, list, and restore snapshots over RCON:

```
> save snapshot
snapshot ok: snap-20260615T141503Z-7c9e2ab04d11 (sha=ab12cd34ef56)
> save list
snap-20260615T141503Z-7c9e2ab04d11  age=42s  sha=ab12cd34ef56
> save restore snap-20260615T141503Z-7c9e2ab04d11
```

A `save restore` takes a pre-restore snapshot first, swaps the world in, and relaunches Bellwright. Use the snapshot path — not a raw file copy — to move a world between servers; [docs/WORLDS.md](docs/WORLDS.md) explains why.

### ♻️ Process supervision
HearthServer runs the Bellwright dedicated process and watches it with a heartbeat watchdog, so a wedged or crash-looping game is detected and surfaced instead of silently hanging.

### 🚫 Bans, schedule, and audit
Bans, the restart schedule, and an admin audit log are persisted in a local SQLite store, so they survive restarts and reinstalls.

### 🧩 Mods
Hearth loads mods through UE4SS on both the host and the client. Hearth's own mods are `bw_host` (host, swaps in the IP/port net driver and opens the listen world) and `HearthConnect` (client, issues the direct join). `HearthConnect` installs with the Hearth app; `bw_host` ships in the host package from [HumanGenome/HearthServer](https://github.com/HumanGenome/HearthServer).

## Install

### Official hosting
[SurvivalServers.com](https://www.survivalservers.com/services/game_servers/bellwright/?utm_source=github&utm_medium=readme_install&utm_campaign=hearth) runs Bellwright servers with Hearth already installed.

### Players
1. Download `HearthSetup-latest.exe` from the [latest release](https://github.com/HumanGenome/Hearth/releases/latest).
2. Run the installer. Windows SmartScreen may warn because the installer is not code-signed yet; choose **More info** then **Run anyway**. Hearth installs per-user — no admin rights required.
3. Open Hearth, add the server address, select or create a character, and click **Connect**.

Hearth checks for launcher updates automatically on launch — you only install once.

### Self-hosted servers
The whole server side is open source at
[HumanGenome/HearthServer](https://github.com/HumanGenome/HearthServer): the
supervisor, the `bw_host` and `bw_fog` mods, the signature packs, the native
helpers and the launch script. Its release page ships
`HearthServer-Host-Windows-x64-v<version>.zip`, a complete host package.
Extract it on a Windows machine (no GPU needed), install Bellwright with a Steam
account that owns it, edit `HearthServer\appsettings.json`, and run
`host-instance.ps1 -GameRoot <your Bellwright folder>`. Players join through
the Hearth app with your ip and port. The step-by-step guide, ports, RCON and
admin details are in that repository's README and `docs/ADMIN.md`.

The client (this app and `HearthConnect`) is closed source.

## Releases

This repo publishes the launcher only:

- `HearthSetup-<version>.exe` — installer for players
- `HearthSetup-latest.exe` — stable download URL that always points at the most recent installer
- `Hearth-Launcher-Windows-x64-v<version>.zip` — portable launcher build
- `checksums-launcher.txt` — hashes for the launcher assets

The host package (`HearthServer-Host-Windows-x64-v<version>.zip`) and the supervisor-only build live on the [HearthServer release page](https://github.com/HumanGenome/HearthServer/releases/latest).

Source archives are generated by GitHub automatically for tags.

## Source

Hearth's public source is split across two repos:

- **Hearth** (this repo) — the client side: the desktop launcher players install, plus the public downloads and documentation.
- **[HearthServer](https://github.com/HumanGenome/HearthServer)** — the server supervisor that runs next to Bellwright. Open source, MIT.

The host-side UE4SS mod that makes Bellwright accept direct IP connections is in neither repo and is not published — see [Self-hosted servers](#self-hosted-servers).

## Community Note

Hearth is a community project and is not affiliated with or endorsed by the developers of Bellwright.

## License

See [LICENSE](LICENSE).

## Credits

- [UE4SS](https://github.com/UE4SS-RE/RE-UE4SS) — Unreal Engine scripting and modding framework
- [Avalonia](https://avaloniaui.net/) — .NET UI framework used by the Hearth launcher
