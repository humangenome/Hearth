<p align="center">
  <img src="docs/img/hearth-lockup-dark.png" alt="Hearth" width="460">
</p>

# Hearth

[![Platform](https://img.shields.io/badge/Platform-Windows_10%2F11-blue.svg)](#install)
[![Game](https://img.shields.io/badge/Game-Bellwright-darkgreen.svg)](https://store.steampowered.com/app/1812450/)
[![Server Source](https://img.shields.io/badge/Server_Source-HearthServer-brightgreen.svg)](https://github.com/HumanGenome/HearthServer)

Hearth gives **Bellwright** IP/port dedicated-server multiplayer: players join through the Hearth launcher, hosts run HearthServer next to Bellwright, and the world stays on the server with snapshots, rollback, admin tools, server query, and scheduled restarts. No Steam invite, no host-must-be-online, no peer-to-peer session.

Every player must install Hearth to join a Hearth server. Stock Bellwright cannot connect to Hearth servers directly.

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

### 🔁 Snapshots and rollback
The server snapshots the world on auto-save, and admins can trigger, list, and restore snapshots over RCON:

```
> save snapshot
snapshot ok: snap-20260615T141503Z-7c9e2ab04d11 (sha=ab12cd34ef56)
> save list
snap-20260615T141503Z-7c9e2ab04d11  age=42s  sha=ab12cd34ef56
> save restore snap-20260615T141503Z-7c9e2ab04d11
```

A `save restore` takes a pre-restore snapshot first, swaps the world in, and relaunches Bellwright.

### ♻️ Process supervision
HearthServer runs the Bellwright dedicated process and watches it with a heartbeat watchdog, so a wedged or crash-looping game is detected and surfaced instead of silently hanging.

### 🚫 Bans, schedule, and audit
Bans, the restart schedule, and an admin audit log are persisted in a local SQLite store, so they survive restarts and reinstalls.

### 🧩 Mods
Hearth loads mods through UE4SS on both the host and the client. Hearth's own mods — `bw_host` (host, swaps in the IP/port net driver and opens the listen world) and `HearthConnect` (client, issues the direct join) — ship with the runtime, and self-hosters can drop their own Lua or native mods into the UE4SS `Mods` folder. See [HearthServer](https://github.com/HumanGenome/HearthServer) for the mod layout.

## Install

### Managed hosting
The easiest option is a [SurvivalServers.com Bellwright server](https://www.survivalservers.com/services/game_servers/bellwright/?utm_source=github&utm_medium=readme_install&utm_campaign=hearth) (official hosting): the complete Hearth server runtime comes preinstalled and the ports are already configured.

### Players
1. Download `HearthSetup-latest.exe` from the [latest release](https://github.com/HumanGenome/Hearth/releases/latest).
2. Run the installer. Windows SmartScreen may warn because the installer is not code-signed yet; choose **More info** then **Run anyway**. Hearth installs per-user — no admin rights required.
3. Open Hearth, add the server address, select or create a character, and click **Connect**.

Hearth checks for launcher updates automatically on launch — you only install once.

### Self-hosted servers
1. Download `Hearth-Server-Windows-x64-v<version>.zip` from the [latest HearthServer release](https://github.com/HumanGenome/HearthServer/releases/latest). This archive is the **supervisor** build — `HearthServer.exe`, its .NET runtime, and `HearthSaveGuard.exe`.
2. Extract it to a stable folder, for example `C:\Hearth\`.
3. Install the Bellwright dedicated server files under the folder set in `appsettings.json` — install with SteamCMD (app `1812450`). HearthServer launches the game from that folder; it does not ship the game.
4. Edit `appsettings.json` (server name, ports, `RconPassword`).
5. Forward/open the gameplay port (UDP), query port (UDP), RCON port (TCP), and admin HTTP port (TCP). The gameplay UDP port needs a Windows Defender inbound allow rule, or players can't reach the listen socket.
6. Run `HearthServer.exe`.

The supervisor archive does not include Hearth's host-side UE4SS runtime or the
Engine.ini templates. Those are what let a Hearth client actually join the world,
and they currently ship only with the managed hosting runtime — a supervisor-only
self-host will start Bellwright but Hearth clients will not be able to connect.
Track [HearthServer](https://github.com/HumanGenome/HearthServer) for the host
runtime package.

Full server setup, settings, ports, RCON commands, and build instructions live in [HumanGenome/HearthServer](https://github.com/HumanGenome/HearthServer).

## Releases

This repo publishes the launcher only:

- `HearthSetup-<version>.exe` — installer for players
- `HearthSetup-latest.exe` — stable download URL that always points at the most recent installer
- `Hearth-Launcher-Windows-x64-v<version>.zip` — portable launcher build
- `checksums-launcher.txt` — hashes for the launcher assets

The dedicated server build (`Hearth-Server-Windows-x64-v<version>.zip`) lives on the [HearthServer release page](https://github.com/HumanGenome/HearthServer/releases/latest).

Source archives are generated by GitHub automatically for tags.

## Source

Hearth is split into two repos:

- **Hearth** (this repo) — the client side: the desktop launcher players install, plus the public downloads and documentation.
- **[HearthServer](https://github.com/HumanGenome/HearthServer)** — the dedicated server hosts run next to Bellwright. Open source.

Players only need this repo's releases; hosts run the server from HearthServer's.

## Community Note

Hearth is a community project and is not affiliated with or endorsed by the developers of Bellwright.

## License

See [LICENSE](LICENSE).

## Credits

- [UE4SS](https://github.com/UE4SS-RE/RE-UE4SS) — Unreal Engine scripting and modding framework
- [Avalonia](https://avaloniaui.net/) — .NET UI framework used by the Hearth launcher
