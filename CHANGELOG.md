# Changelog

## 0.1.1

Raziel WindowsGSM compatibility update.

- Updated compatibility for Raziel7893/WindowsGSM v1.25.2.1.
- Switched Embedded Console handling to the live `AllowsEmbedConsole` value Raziel sets before startup.
- Added a guarded update path for missing Steam credentials or a failed SteamCMD process.
- Kept the firewall cleanup aligned with Raziel's automatic application rule.

## 0.1.0

Initial Farming Simulator 22 support.

- Added SteamCMD install and update support for App ID `1248130` using a licensed Steam account.
- Added startup for the official `dedicatedServer.exe` manager.
- Added WindowsGSM embedded console output.
- Added clean CTRL+C shutdown with sensible fallbacks.
- Added `steam_appid.txt` creation for unattended Steam server starts.
- Disabled the broad automatic WindowsGSM firewall exception for the server manager.
- Added setup and firewall notes.
