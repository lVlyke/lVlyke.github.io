# Stellar Mod Loader

Stellar is a mod manager for Windows and Linux that supports games including Fallout, Oblivion, Skyrim, and Starfield. Formerly known as Starfield Mod Loader.

# Features

* **Add**, **re-order**, **rename**, **disable** and **remove** your mods and plugins.
* **Multiple profiles** enable quick switching between different games and mod loadouts.
* **Base profiles** allows for easily syncing mods between different devices, or VR and non-VR versions of games.
* **Root mod** support enables management of ENBs, script extenders, and other injectors.
* **Custom game support** allows Stellar to work with many games.
* **Cross-platform**: Native clients for Windows and Linux (including Steam Deck).
* Per-profile management of config files, save files, and archive invalidation.
* Support for FOMOD installers.
* Support for **Steam**, **UWP (Game Pass)** and other versions of games.

![Stellar Mod Loader](/docs/app1.png)

# Releases

Releases can be found here:

&nbsp;&nbsp;&nbsp;&nbsp;[Stellar releases](https://github.com/lVlyke/stellar-mod-loader/releases)

# Supported Games
Stellar currently has built-in support the following games:

* **Elder Scrolls IV: Oblivion**
* **Elder Scrolls IV: Oblivion Remastered**
* **Elder Scrolls V: Skyrim LE**
* **Elder Scrolls V: Skyrim SE**
* **Elder Scrolls V: Skyrim VR**
* **Fallout 3**
* **Fallout 4**
* **Fallout 4 VR**
* **Fallout: New Vegas**
* **Starfield**

Many other games are supported with **[custom game support](#custom-games)**.

# Installation

> [!IMPORTANT]
> [7-Zip](https://www.7-zip.org/) must be installed to use Stellar.

To install Stellar, simply download the latest release from the [releases page](https://github.com/lVlyke/stellar-mod-loader/releases) and extract the archive to a folder of your choice.

> [!NOTE]
> If you are using Ubuntu or an Ubuntu-based Linux distribution, you will need to run the included `install-apparmor-profile.sh` script in order to create an AppArmor profile for Stellar. See [this Ubuntu blog post](https://ubuntu.com/blog/ubuntu-23-10-restricted-unprivileged-user-namespaces) for more information.

# Using Stellar

There are a few different ways to learn how to use Stellar:
* View the guided in-app help in Stellar itself under **Help > Show Help Topics**.
* You can also view the in-app help [directly in your browser](TODO).
* Or you can read the [usage guide](/docs/usage-guide.md).

# Troubleshooting

## Common issues

### Symlinks are not enabled <a name="symlinks"></a>

If you get a warning about symlinks not being enabled when creating or editing a profile, you need to enable symlink permissions.

To enable symlinks in Windows, you can either A) enable Windows Developer Mode by going the Windows "Settings" app, select "For developers", and then enable "Developer Mode", or B) run Stellar as administrator (not recommended). Once enabled, Stellar should now be able to use symlinks.

### The app sits on the "Verifying Profile..." loading screen for a long time during startup

This can happen when very large profiles are activated. If profile verification is taking too long, you can disable verification on app startup via the menu bar under **File > Preferences**.

### **(Linux)** Stellar won't run with error "The SUID sandbox helper binary was found, but is not configured correctly."

If you are using Ubuntu or a related distribution, you will need to run the included `install-apparmor-profile.sh` script in order to create an AppArmor profile for Stellar. See [this Ubuntu blog post](https://ubuntu.com/blog/ubuntu-23-10-restricted-unprivileged-user-namespaces) for more information.

### **(Linux)** Some mods are not loading/strange behavior when loading some mods

This can be fixed by enabling **Normalize mod file path** for the app. See [this section](#normalize-mod-file-path) for more information.

### **(Linux)** Mods are not loading when using a script extender like SFSE

You must set the `STEAM_COMPAT_DATA_PATH` environment variable to use the game's Proton prefix. Stellar will do this automatically when creating Steam shortcuts.

### (Starfield) My mods are not loading

First, make sure you have added the following lines to your `StarfieldCustom.ini` file:

```ini
[Archive]
bInvalidateOlderFiles=1
sResourceDataDirsFinal=
```

If mods still are not working, you may also need to also add these lines to your `StarfieldPrefs.ini` file.

If you are using the game's installation `Data` folder as your **Mod Base Directory**, make sure you delete the `Data` folder at `Documents/My Games/Starfield`, otherwise your mods will not be detected by the game. The game will automatically create this folder on game start and when you take in-game screenshots. To change this behavior, you can add the following lines to your `StarfieldCustom.ini` and `StarfieldPrefs.ini` files to disable MotD and change your screenshots folder:

```ini
[General]
bEnableMessageOfTheDay=0
```

```ini
[Display]
sPhotoModeFolder=Photos
```

## Report an issue

If you run into a problem, please check the [issues page](https://github.com/lVlyke/stellar-mod-loader/issues) to see if your question has been answered, or create a new issue if you have a new bug to report.

If you have a suggestion for a new feature or a new game to support, feel free to open an issue for your request.

# Devs - Building and testing

To build and run the app for testing and development, ensure you have Node and NPM installed on your machine and run `npm install` and `npm run start`.

To build a packaged release, run `npm run package:release` for the current platform or `npm run package:release:all` for all supported platforms.

## Setting up dev environment with nix

Developing with a conventional node installation is fine. Optionally you can let nix handle the dev environment for you. nix allows to keep the environment reproducible (and makes developing on NixOS possible). See (discussion in PR #23)[https://github.com/lVlyke/stellar-mod-loader/pull/23]. This is only tested on Linux. To use nix dev shells you need to install and setup:
- [The nix package manager](https://nixos.org/download/)
- [direnv](https://direnv.net/), optimally with [a shell hook](https://direnv.net/docs/hook.html)
- [nix-direnv](https://github.com/nix-community/nix-direnv)
- run `direnv allow` to allowlist the shell.nix.
