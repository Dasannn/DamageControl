# Damage Control

Restores vanilla explosion damage inside areas your players buy, on servers where explosions are globally disabled.

If you run EssentialsProtect with explosions turned off, nothing blows up anywhere. That stops grief and automatic explosion farms, but it also takes away something players enjoy. Damage Control gives it back in one place at a time: a player buys an area, places it, and inside that square explosions behave exactly like vanilla again. Everywhere else, your global protection stays untouched.

## How it works

1. A player buys an area item with `/dc buy`, paying through Vault.
2. They place the item where they want the area. The block is never placed; the area is created around that point and becomes active immediately.
3. Inside the area, explosions break blocks and drop items exactly as vanilla would.
4. Outside the area, nothing changes.

Areas are between 1×1 and 16×16 blocks, cover the full height of the world, and never overlap. Anyone can trigger an explosion inside any area, including players who do not own it — deciding who may build is your land protection plugin's job, not this one's.

## Requirements

| | |
|---|---|
| Server | Paper 26.2 |
| Java | 25 |
| Required | [Vault](https://www.spigotmc.org/resources/vault.34315/) and an economy plugin |

## Optional integrations

All of these are soft dependencies. If a plugin is missing, Damage Control starts and runs without it.

| Plugin | What it does |
|---|---|
| Towny | Areas cannot be created where the player lacks build permission. Towny's explosion setting is respected. Claims cannot be made over an existing area. |
| WorldGuard | Region flags are respected. A region denying `tnt` stops TNT inside an area without affecting creepers. |
| EssentialsProtect | The global block this plugin is built to override. Outside areas it keeps working exactly as before. |
| LuckPerms | Per-player area limits through permission nodes. |

## Installation

1. Download the latest `DamageControl-<version>.jar` from [Releases](../../releases).
2. Drop it into your `plugins/` folder.
3. Restart the server.
4. Edit `plugins/DamageControl/config.yml` and reload with `/dc admin reload`.

Only the `.jar` goes into `plugins/`. Each release also ships a `.jar.sha256` file, which you never install: it is the checksum the in-game updater uses to verify a download before accepting it, and it lets you confirm by hand that the file you downloaded is the one that was published.

```powershell
(Get-FileHash DamageControl-1.0.0.jar -Algorithm SHA256).Hash.ToLower()
```

Compare that against the contents of `DamageControl-1.0.0.jar.sha256`. If they differ, do not install the file.

## Commands

Run `/dc help` in game for the list, filtered to what you can actually use.

| Command | What it does |
|---|---|
| `/dc buy [XxZ]` | Buy an area item. Without a size, opens the purchase menu. |
| `/dc refund` | Return an unplaced item and get part of the price back. |
| `/dc list` | Your areas, with world, size and coordinates. |
| `/dc info <id>` | Area detail: owner, corners, date and price paid. |
| `/dc show <id>` | Draws the area borders with particles. |
| `/dc remove <id>` | Delete one of your areas. No refund. |
| `/dc help` | The command list, in your language. |

Administration lives under `/dc admin`: `list`, `info`, `near`, `give`, `remove`, `reload`, `db status` and `update`.

## Permissions

| Node | Grants | Default |
|---|---|---|
| `damagecontrol.use` | Viewing and visualising your own areas | everyone |
| `damagecontrol.buy` | Buying area items | everyone |
| `damagecontrol.refund` | Refunding unplaced items | everyone |
| `damagecontrol.place` | Placing area items | everyone |
| `damagecontrol.remove` | Deleting your own areas | everyone |
| `damagecontrol.limit.areas.<n>` | Raises the area limit to `<n>` | — |
| `damagecontrol.bypass.limit` | Ignores the area limit | op |
| `damagecontrol.bypass.claim` | Claiming over an existing area with Towny | op |
| `damagecontrol.admin` | Every administration command | op |

## Languages

Spanish and English ship in the same jar. Set `language` in `config.yml` for the server default, and leave `per-player-language` on to follow each player's own client setting.

## Storage

Areas are stored locally in `areas.json`, which is the source of truth. A MySQL mirror is optional and off by default: turn it on in `config.yml` and fill in your own connection details. If the database goes down, the plugin keeps working and writes are queued.

No credentials ship with the plugin. The `host`, `database`, `user` and `password` fields are empty until you fill them in on your own server.

## Updates

The plugin checks this repository's releases and tells administrators when a new version is out. `/dc admin update download` verifies the release's SHA-256 checksum before accepting the file, and drops it into your server's update folder to be applied on the next restart. A file whose checksum does not match is deleted and never installed.

## Support

Open an [issue](../../issues) with your server version, the plugin version, and the relevant part of your console log.

## License

Copyright © 2026 Dasannn. All rights reserved.

This plugin is distributed as a compiled binary for use on Minecraft servers. It is **not** open source: see [LICENSE](LICENSE) for what you may and may not do with it.
