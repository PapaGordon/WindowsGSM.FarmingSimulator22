# WindowsGSM.FarmingSimulator22

WindowsGSM plugin for running a Farming Simulator 22 dedicated server on Windows.

This first version sticks close to the way GIANTS ships the server. WindowsGSM installs and updates the Steam copy, starts `dedicatedServer.exe`, shows its console output and stops it cleanly with CTRL+C. The actual game settings still live in the Farming Simulator web panel.

## What it does

- Installs and updates Farming Simulator 22 through SteamCMD.
- Uses Steam App ID `1248130`.
- Requires a Steam account that owns Farming Simulator 22. Anonymous SteamCMD login does not work for this game.
- Starts the official `dedicatedServer.exe` server manager.
- Supports the WindowsGSM embedded console.
- Stops the server manager with CTRL+C before using any forced fallback.
- Creates `steam_appid.txt` with the Farming Simulator 22 App ID. This avoids the Steam launch confirmation that otherwise gets in the way on unattended Steam servers.
- Removes WindowsGSM's broad automatic firewall exception for `dedicatedServer.exe` before it starts listening.
- Does not create firewall rules on its own.

## Requirements

- WindowsGSM
- Windows Server or Windows 10/11 x64
- A dedicated Steam account with its own Farming Simulator 22 license
- Administrator rights for WindowsGSM if the firewall safety check is enabled

The server needs its own game license. Do not use the same Steam account for the dedicated server and a player who is supposed to join it at the same time.

## Install the plugin

1. Download the latest release.
2. Put the complete `FarmingSimulator22.cs` folder into `<WindowsGSM>\plugins\`.
3. Reload plugins or restart WindowsGSM.
4. Add **Farming Simulator 22 Dedicated Server**.
5. Use **Set Account** in the WindowsGSM install window and enter the Steam account that owns LS22.
6. Install the server.
7. Start it once and open the URL printed by `dedicatedServer.exe`.

Steam Guard can still ask for a code during installation or updates. That is normal for a non-anonymous SteamCMD login.

## Configuration

Farming Simulator 22 keeps its actual server settings in the official web panel. The normal defaults are:

| Setting | Default |
| --- | --- |
| Game port | `10823` |
| Player slots | up to `16` |
| Web panel | usually `8080` for HTTP |
| Server manager | `dedicatedServer.exe` |
| Game process | `x64\FarmingSimulator2022Game.exe` |

The **Port**, **Server Name**, **Map** and **Max Players** fields in WindowsGSM are not written into the Farming Simulator configuration in v0.1.0. Change the live server settings in the GIANTS web panel for now.

## Firewall

The plugin deliberately removes the unrestricted WindowsGSM application exception for `dedicatedServer.exe`.

Create narrow rules yourself instead:

- Allow the Farming Simulator game port required by your server configuration.
- Keep the web panel private. Prefer localhost, LAN or WireGuard/VPN access instead of publishing the admin panel to the internet.
- If you change the game or web port in the Farming Simulator configuration, adjust the firewall rule as well.

The plugin leaves your manual firewall rules alone.

## Embedded console

With **Embed Console** enabled, output from `dedicatedServer.exe` is shown inside WindowsGSM. The native console stays hidden.

On shutdown the plugin sends CTRL+C first because that is the shutdown method the GIANTS server manager itself asks for. If that fails, it tries to close the window and only then falls back to terminating the process.

## Notes about the Steam version

Farming Simulator 22 does not have a separate anonymous Steam dedicated-server app. The dedicated server files are part of the normal game installation, so WindowsGSM has to install App ID `1248130` with an account that owns the game.

The plugin writes `steam_appid.txt` into the server root. This is a common workaround for the Steam confirmation dialog when `dedicatedServer.exe` starts the game process. It does not replace the required game license.

## First test

For the first run I would check these points before treating the plugin as finished:

- Install completes with the dedicated Steam account.
- `dedicatedServer.exe` starts from WindowsGSM.
- The embedded console shows the web panel URLs and the CTRL+C shutdown line.
- The web panel can start `FarmingSimulator2022Game.exe` without a Steam confirmation popup.
- The game becomes joinable on the configured port.
- WindowsGSM Stop shuts both the web manager and game server down cleanly.
- No broad `dedicatedServer.exe` firewall exception remains afterwards.

## Project

- Source: https://github.com/PapaGordon/WindowsGSM.FarmingSimulator22
- WindowsGSM: https://github.com/WindowsGSM/WindowsGSM
- Community: https://mefriendos.de

This is an independent community plugin and is not affiliated with GIANTS Software, Valve or WindowsGSM.

## License

MIT. See `LICENSE`.
