<p align="center">
  <img src="FarmingSimulator22.cs/FarmingSimulator22.png" alt="Farming Simulator 22" width="128">
</p>

<h1 align="center">WindowsGSM.FarmingSimulator22</h1>

<p align="center">
  Run a Farming Simulator 22 dedicated server with WindowsGSM.
</p>

<p align="center">
  <a href="https://github.com/PapaGordon/WindowsGSM/releases/latest"><img src="https://img.shields.io/badge/WindowsGSM-MeFriendos%20latest-38CDD4" alt="Latest MeFriendos WindowsGSM"></a>
  <a href="CHANGELOG.md"><img src="https://img.shields.io/badge/version-0.1.3-80B918" alt="Version 0.1.3"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
</p>

This plugin installs, updates, starts and stops a Farming Simulator 22 dedicated server from WindowsGSM. The server settings themselves, like name, password, port, slots, map and mods, stay in the official GIANTS web panel.

**Using our [MeFriendos WindowsGSM](https://github.com/PapaGordon/WindowsGSM)?** Then you don't need this plugin. Farming Simulator 22 is built in there, with the same code. Just pick **Farming Simulator 22 Dedicated Server** under **Install**. This plugin is for other WindowsGSM builds.

## What it does

- Installs and updates Farming Simulator 22 (Steam app `1248130`) with a Steam account that owns the game.
- Starts the GIANTS server manager `dedicatedServer.exe`.
- Checks before every start that the game and `dedicatedServer.xml` are there, so an incomplete installation doesn't start.
- Creates `steam_appid.txt`, so Steam doesn't ask for confirmation when the server starts the game.
- Shows the server manager's output in **Embed Console**.
- Shuts the server down with CTRL+C first and only ends the process if nothing else works.
- Never changes your GIANTS settings, savegames or mods.

## What you need

- 64-bit Windows and a WindowsGSM build with **Set Account** for SteamCMD. We test with the current [MeFriendos WindowsGSM](https://github.com/PapaGordon/WindowsGSM/releases/latest).
- A Steam account that owns Farming Simulator 22. There's no separate server download, the server comes with the normal game. If someone wants to play with that account while the server runs, use a separate account and license for the server.

The plugin signs in through WindowsGSM's own SteamCMD login. Encrypted Steam passwords (`steamPassEncrypted`) need MeFriendos WindowsGSM v1.25.4.2 or newer, and WindowsGSM has to run as the Windows user that encrypted the password. [Steam account and password](https://github.com/PapaGordon/WindowsGSM/blob/main/docs/operations/steam-credentials.md) Other builds only read the plain `steamPass` field. Updating this plugin can't change that.

## Install

1. Download the ZIP from the [latest release](https://github.com/PapaGordon/WindowsGSM.FarmingSimulator22/releases/latest).
2. Copy the whole `FarmingSimulator22.cs` folder into `<WindowsGSM>\plugins\`.
3. Click **Reload Plugins** or restart WindowsGSM.
4. Pick **Farming Simulator 22 Dedicated Server**.
5. Click **Set Account** and sign in with the Steam account that owns the game. Steam Guard may ask for a code.
6. Click **Install**.
7. Start the server in WindowsGSM.
8. Open the web panel address that `dedicatedServer.exe` shows and set up your game there.
9. Open the game ports in the Windows firewall and on your router.

## Settings

Everything that matters for the game is set in the GIANTS web panel. WindowsGSM doesn't pass ports, player slots or a map to the server. Its **Port** and **Max Players** fields are only for display and to keep several servers apart: each new Farming Simulator 22 server in WindowsGSM gets a port 1 higher than the previous one. The web panel's own address and port are in `dedicatedServer.xml`.

| | Default |
| --- | --- |
| Game port | `10823` |
| Web panel | See `dedicatedServer.xml` |
| Port step between servers in WindowsGSM | `1` |
| Server status in WindowsGSM | Not available, there's no query |

## Updating

1. Stop the server.
2. Back up the savegame, mods and server settings.
3. Click **Update** in WindowsGSM and enter a Steam Guard code if Steam asks for one.
4. Start the server again.
5. Check the web panel and the server log before players join.

## Console and stopping

With **Embed Console** on, the server manager's output appears in WindowsGSM and its own window stays hidden.

**Stop** works in three steps:

1. Send CTRL+C and wait up to 30 seconds.
2. Close the server manager's window and wait up to 10 seconds.
3. Only if the server still runs, end the process.

## Ports and firewall

The plugin doesn't create or remove firewall rules. Use WindowsGSM's **Auto Firewall** if your build has it, or open the ports yourself. Router port forwarding is always up to you.

- Only open the game ports your server really uses. By default that's port `10823`.
- Keep the web panel private. Reach it from your local network or a VPN like WireGuard, not from the internet.
- When you change the port in the GIANTS settings, change your firewall rules too.

## Troubleshooting

**WindowsGSM asks for a Steam account.** That's expected. Farming Simulator can't be downloaded anonymously. Use **Set Account** with an account that owns the game.

**Steam Guard asks for a code.** That's normal for an account login through SteamCMD. Enter the code and WindowsGSM continues.

**"File not found" for `dedicatedServer.exe` or `x64\FarmingSimulator2022Game.exe`, or `dedicatedServer.xml` is missing.** The installation is incomplete. Run **Update** with validation or reinstall.

**"steam_appid.txt could not be created".** Check the write permissions of the server folder.

**Players can't connect.** Check the game port in the web panel, your firewall rules and your router forwarding.

**The web panel isn't reachable from outside.** That's how it should be. Use your local network or a VPN.

## Links

- [Releases](https://github.com/PapaGordon/WindowsGSM.FarmingSimulator22/releases)
- [MeFriendos WindowsGSM](https://github.com/PapaGordon/WindowsGSM)
- [Farming Simulator](https://www.farming-simulator.com/)
- [MeFriendos](https://mefriendos.de)

This is a community plugin. It's not affiliated with or endorsed by GIANTS Software, Valve or the WindowsGSM project.

## License

[MIT License](LICENSE). You can use, change and share the plugin, also commercially, as long as the copyright and license notice stay with it.
