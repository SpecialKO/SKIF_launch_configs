This repository facilitates collaboration for the managed custom launch configs used within the Special K Injection Frontend (SKIF).
Note that using a launch config normally bypasses the typical launch process of its digital distribution platform, meaning features such as cloud synchronization etc is often skipped entirely.

## Instructions

1. Edit the `lc.json` file with the desired additions/changes.
2. Once the edits are done, make a pull request of those edits.
3. Allow time for someone in the Special K team to validate and approve the changes.
4. Once the changes have been merged, they will be pushed out to all users over the next week or so.

## Format

The JSON is structured by `<platform-id>` -> `<platform-app-id>` -> `<array-of-custom-launch-configs>` -> `<individual-launch-config>`.

This example defines a custom `-nostartupmovie` launch configuration for the Steam copy of Mirror's Edge:

```json
{
  "Steam": {
    "17410_Name": "Mirror's Edge",
    "17410": [
      {
        "Desc": "Skip intro videos",
        "Exe": "Binaries/MirrorsEdge.exe",
        "Args": "-nostartupmovies",
        "Dir": "Binaries"
      }
    ]
  }
}
```

* `Steam` is the platform identifier (`<platform-id>`). See [Supported platforms](#supported-platforms) for more details about that.

* `17410_Name` is the human-readable name of the game. This key is not actually used &mdash; it is only present to make it easier for editors to identify games. The name of this key should always be `<platform-app-id>_Name`.

* `17410` is the unique platform app ID (`<platform-app-id>`) of the game. In the case of Steam this is typically just known as the "Steam App ID".
  * It can be retrieved from SKIF (right click -> Developer -> **Copy app ID** provided Developer mode is turned on).
     * This is the recommended option as it supports all platforms.
  * For Steam games, it can also be retrieved from the Steam storefront page link itself.
     * e.g. https://store.steampowered.com/app/17410/Mirrors_Edge/
  * Or from the third-party SteamDB resource.
    * e.g. https://steamdb.info/app/17410/info/

* `Desc` is the name/description that SKIF will use when listing it as an option.
  * This is used in the context menu of games as well as in the "Disable Special K" menu.
  * Note that SKIF will omit listing launch options in the "Disable Special K" menu that share the same executable as another option.

* `Exe` holds the relative path to the executable to start.
  * The path is relative to the install folder of the game.

* `Args` holds the command-line arguments to start the executable with.
  * These are known as the "launch options" in the Steam client and elsewhere.

* `Dir` holds the relative working directory used for the launched executable. This can be empty, in which case the folder of the executable is used instead.
  * The path is relative to the install folder of the game.

## Supported platforms

All platforms in SKIF parses and can list custom launch options, though its usefulness is primarily limited to Steam, GOG, and Custom games (`lc_user.json`).
The platform identifier (`<platform-id>`) is the same as shown as the "Platform" label in the app.

| Platform ID         | State of support                                                                                                           |
| ------------------: | :------------------------------------------------------------------------------------------------------------------------- |
| `Steam`             | Best supported platform, with few known problems or challenges when using custom launch configs.                           |
| `GOG`               | Partially supported platform, though limited to launches outside of the GOG Galaxy client for now.                         |
| `Epic`              | Very little, as custom launch configs can only be used with (DRM-free) games that allows skipping the Epic Games Launcher. |
| `Xbox`              | Extremely little use, due to the special lauch requirements of these games.                                                |
| `Custom`            | Any manually added game, though these entries should be done in the separate `lc_user.json` file.                          |

## Examples

This example defines a few different launch configurations for a few Steam games:

* an intro skip option for [Mirror's Edge](https://steamdb.info/app/17410/)
* a launcher skip for [Doom Eternal](https://steamdb.info/app/782330/)
* as well as two custom launch options for [Half-Life 2](https://steamdb.info/app/220/)

```json
{
  "Steam": {
    "17410_Name": "Mirror's Edge",
    "17410": [
      {
        "Desc": "Skip intro videos",
        "Exe": "Binaries/MirrorsEdge.exe",
        "Args": "-nostartupmovies",
        "Dir": "Binaries"
      }
    ],
    "782330_Name": "Doom Eternal",
    "782330": [
      {
        "Desc": "Skip launcher",
        "Exe": "DOOMEternalx64vk.exe",
        "Args": "",
        "Dir": ""
      }
    ],
    "220_Name": "Half-Life 2",
    "220": [
      {
        "Desc": "Skip intro videos",
        "Exe": "hl2.exe",
        "Args": "-game hl2 -steam -novid",
        "Dir": "bin"
      },
      {
        "Desc": "Enable the console",
        "Exe": "hl2.exe",
        "Args": "-game hl2 -steam -console",
        "Dir": "bin"
      }
    ]
  }
}
```
