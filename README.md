<p align="center">
  <img src="FarmingSimulator22.cs/FarmingSimulator22.png" alt="Farming Simulator 22" width="128">
</p>

<h1 align="center">WindowsGSM.FarmingSimulator22</h1>

<p align="center">
  MeFriendos build for running a Farming Simulator 22 dedicated server with WindowsGSM.
</p>

<p align="center">
  <a href="https://github.com/Raziel7893/WindowsGSM/releases/tag/v1.25.2.1"><img src="https://img.shields.io/badge/WindowsGSM-Raziel%20v1.25.2.1-38CDD4" alt="Raziel WindowsGSM v1.25.2.1"></a>
  <a href="CHANGELOG.md"><img src="https://img.shields.io/badge/version-0.1.1-80B918" alt="Version 0.1.1"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
</p>

This plugin adds Farming Simulator 22 dedicated-server support to WindowsGSM. It handles installation, updates, startup and shutdown, while the actual server settings stay in the official GIANTS web panel.

FS22 uses the normal licensed Steam game files, so installation needs a Steam account that owns Farming Simulator 22.


## Features

- Installs and updates Farming Simulator 22 through SteamCMD.
- Uses Steam App ID `1248130`.
- Uses a normal Steam account instead of anonymous SteamCMD login.
- Starts the official `dedicatedServer.exe` server manager.
- Checks that `FarmingSimulator2022Game.exe` and `dedicatedServer.xml` are present before startup.
- Creates `steam_appid.txt` with the correct App ID when needed.
- Supports the WindowsGSM Embedded Console using the live state Raziel passes through `AllowsEmbedConsole`.
- Sends CTRL+C first for a clean shutdown before using fallback methods.
- Removes WindowsGSM's broad automatic firewall application exception for the exact `dedicatedServer.exe` path.
- Leaves targeted manual firewall rules untouched.
- Does not change the GIANTS web-panel configuration automatically.
- Handles/ a missing Steam account or failed SteamCMD update cleanly instead of relying on Raziel's base update path.

## Quick overview

| Setting | Value |
| --- | --- |
| Steam App ID | `1248130` |
| SteamCMD login | Licensed Steam account required |
| Start executable | `dedicatedServer.exe` |
| Game executable | `x64\FarmingSimulator2022Game.exe` |
| Default game port | `10823` |
| Default web panel | `8080` |
| Default max players | `16` |
| Port increment | `1` |
| Query method | None in WindowsGSM |
| Embedded Console | Supported |
| Server configuration | GIANTS web panel |
| Firewall | Manual port rules only |

## Raziel WindowsGSM compatibility

This plugin works with **Raziel7893/WindowsGSM v1.25.2.1** and uses the fork's current plugin API, including **Set Account**, Steam Guard handling and the live **Embed Console** state.

It also removes the broad WindowsGSM application firewall exception for `dedicatedServer.exe` before startup, while leaving your own port-specific rules alone.

## Requirements

- [Raziel7893/WindowsGSM v1.25.2.1](https://github.com/Raziel7893/WindowsGSM/releases/tag/v1.25.2.1) or a compatible WindowsGSM build
- 64-bit Windows
- Administrator rights for WindowsGSM when the firewall safety check needs to remove a broad application exception
- A Steam account with its own Farming Simulator 22 license

Raziel's current WindowsGSM builds use the **.NET 8 Desktop Runtime**. If you are moving from the original WindowsGSM build, install the Desktop Runtime rather than only the normal .NET 8 runtime.

The dedicated server needs its own Farming Simulator 22 license. Do not plan to use the same Steam account for the server and a player who needs to play at the same time.

## Plugin installation

1. Download the latest release archive.
2. Extract the complete `FarmingSimulator22.cs` folder into `<WindowsGSM>\plugins\`.
3. Click **Reload Plugins** or restart WindowsGSM.
4. Add **Farming Simulator 22 Dedicated Server**.
5. In the install window use **Set Account** and enter a Steam account that owns Farming Simulator 22.
6. Run **Install**.
7. Start the server manager.
8. Open the web panel URL shown by `dedicatedServer.exe` and finish the game server setup there.
9. Create the required game-port firewall rule manually.

Steam Guard can ask for a code during installation or updates because FS22 cannot be installed anonymously through SteamCMD.

## Steam account and installation

Farming Simulator 22 does not use a separate anonymous Steam dedicated-server app. The dedicated server files are part of the normal game installation.

WindowsGSM therefore installs App ID:

```text
1248130
```

with the Steam account configured through **Set Account**.

The plugin also makes sure the following file exists in the server root:

```text
steam_appid.txt
```

with:

```text
1248130
```

This helps avoid Steam's launch confirmation getting in the way when the server manager starts the game process. It does not replace the required FS22 license.

## Server configuration

FS22 keeps the actual dedicated-server settings in the GIANTS web panel.

The plugin leaves `dedicatedServer.xml` alone, so changes made in the official panel stay there. WindowsGSM handles installation, updates and the server process; use the GIANTS web panel for the live server settings.


## Ports and firewall

The default game port is:

| Purpose | Port |
| --- | --- |
| Farming Simulator 22 game traffic | `10823` |
| GIANTS web panel | `8080` by default |

The plugin removes the unrestricted WindowsGSM application exception for the exact `dedicatedServer.exe` path before startup.

It does **not** create firewall rules automatically.

For the MeFriendos setup:

- Allow only the game port that the FS22 server actually uses.
- Keep the web panel on localhost, LAN or WireGuard/VPN access.
- Do not expose the admin web panel publicly unless there is a specific reason to do so.
- Adjust the firewall rule when you change the game port in the GIANTS configuration.

Existing manual firewall rules are left untouched.

## Embedded Console and shutdown

When **Embed Console** is enabled, stdout and stderr from `dedicatedServer.exe` are redirected into WindowsGSM and the native console window stays hidden.

When stopping the server, the plugin uses this order:

1. Send CTRL+C and wait up to 30 seconds.
2. Try to close the native server-manager window.
3. Use `Process.Kill()` only if the normal shutdown methods failed.

The forced kill is intentionally the last fallback.

## Updating Farming Simulator 22

1. Stop the server.
2. Back up the savegame and server configuration.
3. Click **Update** in WindowsGSM.
4. Complete Steam Guard authentication if Steam asks for it.
5. Start the server manager again.
6. Check the web panel and server log before opening the server to players.

WindowsGSM updates the game installation. The plugin does not intentionally change savegames, mods or the GIANTS server configuration.

## Troubleshooting

### WindowsGSM asks for a Steam account

That is expected. FS22 cannot be installed through anonymous SteamCMD login. Use **Set Account** with an account that owns Farming Simulator 22.

### Steam Guard asks for a code

That is normal for the licensed Steam account used by SteamCMD. Complete the authentication and let WindowsGSM continue the install or update.

### Startup says dedicatedServer.exe is missing

Run **Update** with validation or reinstall the server files. The plugin will not start an incomplete FS22 installation.

### Startup says FarmingSimulator2022Game.exe is missing

The normal game installation is incomplete. Validate the installation through WindowsGSM/SteamCMD.

### dedicatedServer.xml is missing

Validate the FS22 installation. The plugin expects the normal GIANTS dedicated-server files to be present.

### Players cannot connect

Check the game port configured in the GIANTS web panel and make sure the same port is allowed by a narrow firewall rule and forwarded where required.

### The web panel is not reachable remotely

This can be intentional. For the MeFriendos setup the admin panel should stay private and be accessed through LAN or WireGuard/VPN rather than a public firewall rule.

### WindowsGSM reports that automatic firewall access could not be disabled

Run WindowsGSM as administrator. If an unrestricted `dedicatedServer.exe` application rule exists, remove it manually and keep only the intended port-specific rules.

## Project links

- Source: [PapaGordon/WindowsGSM.FarmingSimulator22](https://github.com/PapaGordon/WindowsGSM.FarmingSimulator22)
- WindowsGSM: [Raziel7893/WindowsGSM](https://github.com/Raziel7893/WindowsGSM)
- Current WindowsGSM release: [v1.25.2.1](https://github.com/Raziel7893/WindowsGSM/releases/tag/v1.25.2.1)
- Farming Simulator: [farming-simulator.com](https://www.farming-simulator.com/)
- Community: [mefriendos.de](https://mefriendos.de)

This is an independent community plugin. It is not affiliated with or endorsed by GIANTS Software, Valve or WindowsGSM.

## License

This MeFriendos plugin is released under the [MIT License](LICENSE).
