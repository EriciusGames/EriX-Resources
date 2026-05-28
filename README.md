# EriX Game Launcher: Platforms, Apps, and Emulators Configuration

This guide provides everything you need to configure gaming platforms, apps, and emulators for the **EriX Game Launcher**. **EriX** uses structured JSON files to define how systems are identified, scanned, and launched.

---

## ✦ Example: `Sony PlayStation Portable (PSP)`

A complete configuration file typically looks like this. A few key rules to keep in mind:

- `fullName` should not include the company name unless it is used as a device name (e.g., `Atari 800`).
- `description` should be a short sentence and omit the trailing period for a cleaner UI.

```json
{
  "id": "psp",
  "name": "PSP",
  "fullName": "PlayStation Portable",
  "details": {
    "company": "Sony",
    "release": "2004-12-12",
    "description": "Sony's iconic handheld console that brought console-quality gaming right into your pocket",
    "type": "handheld"
  },
  "folders": ["psp", "PlayStation Portable"],
  "emulators": {
    "retroarch": [
      {
        "core": "ppsspp",
        "name": "PPSSPP",
        "achievements": true,
        "extensions": ["cso", "iso", "pbp"]
      }
    ],
    "standalone": [
      {
        "id": "ppsspp.default",
        "name": "PPSSPP Default",
        "provider": "PPSSPP",
        "default": true,
        "achievements": true,
        "extensions": ["cso", "iso", "pbp"],
        "intent": {
          "packageName": "org.ppsspp.ppsspp",
          "className": "org.ppsspp.ppsspp.PpssppActivity",
          "action": "android.intent.action.VIEW",
          "category": "android.intent.category.DEFAULT",
          "data": "{FILE_URI}"
        }
      },
      {
        "id": "ppsspp.gold",
        "name": "PPSSPP Gold",
        "provider": "PPSSPP",
        "achievements": true,
        "extensions": ["cso", "iso", "pbp"],
        "intent": {
          "packageName": "org.ppsspp.ppssppgold",
          "className": "org.ppsspp.ppsspp.PpssppActivity",
          "action": "android.intent.action.VIEW",
          "category": "android.intent.category.DEFAULT",
          "data": "{FILE_URI}"
        }
      }
    ]
  }
}
```

---

## 1. Platform Configuration

Defines the overall profile of the gaming system and maps it to compatible apps and emulators.

```json
{
  "id": "psp",
  "name": "PSP",
  "fullName": "PlayStation Portable",
  "details": { ... },
  "folders": ["psp", "PlayStation Portable"],
  "emulators": { ... }
}
```

| Field       | Type       | Required | Description                                                                                                                                                                                                     |
|:------------|:-----------|:--------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `id`        | `string`   | **Yes**  | **Unique identifier** for the platform (e.g., `psp`, `psx`, `n64`).                                                                                                                                             |
| `name`      | `string`   | **Yes**  | **Short and unique name** used as the primary display name in the UI.                                                                                                                                           |
| `fullName`  | `string`   | **Yes**  | Full name of the platform. **Do not include the company name** unless it is part of the device name itself (e.g., use `PlayStation Portable`, but use `Atari 800`).                                             |
| `details`   | `object`   | **Yes**  | Historical and technical metadata. See [Platform Details](#2-platform-details).                                                                                                                                 |
| `folders`   | `string[]` | **Yes**  | List of folder names (relative to the ROMs root folder) that will be scanned for files and games.                                                                                                               |
| `emulators` | `object`   | **Yes**  | Lists of supported [RetroArch](#RetroArch-Cores) and [Standalone](#Standalone-Apps-and-Emulators) apps and emulators.                                                                                           |

---

## 2. Platform Details

Specifies how the platform is presented and categorized within the interface.

```json
{
  "company": "Sony",
  "release": "2004-12-12",
  "description": "Sony's iconic handheld console that brought console-quality gaming right into your pocket",
  "type": "handheld"
}
```

| Field         | Type     | Required | Description                                                                   |
|:--------------|:---------|:--------:|:------------------------------------------------------------------------------|
| `company`     | `string` | **Yes**  | Company that created the hardware (e.g., `Sony`, `Nintendo`).                 |
| `release`     | `string` | **Yes**  | System's official release date in `YYYY-MM-DD` format.                        |
| `description` | `string` | **Yes**  | Brief and punchy summary of the platform's history and significance.          |
| `type`        | `string` | **Yes**  | Hardware category. See [Supported Platform Types](#supported-platform-types). |

### Supported Platform Types

The `type` field must be one of the following values:

* `arcade`: Arcade systems (e.g., `MAME`, `Neo Geo`).
* `console`: Home gaming consoles (e.g., `PlayStation`, `NES`).
* `computer`: Personal computers (e.g., `Amiga`, `Commodore 64`, `Windows`).
* `handheld`: Portable gaming systems (e.g., `Game Boy`, `PSP`).
* `virtual`: Virtual or fantasy consoles (e.g., `PICO-8`).

---

## 3. Apps and Emulators Configuration

### RetroArch Cores

Cores that run within the RetroArch environment. The launcher handles most of the intent complexity for you.

```json
{
  "core": "ppsspp",
  "name": "PPSSPP",
  "achievements": true,
  "extensions": ["cso", "iso", "pbp"]
}
```

| Field          | Type       | Required | Description                                                                           |
|:---------------|:-----------|:--------:|:--------------------------------------------------------------------------------------|
| `core`         | `string`   | **Yes**  | **Unique core name** as identified by RetroArch (e.g., `ppsspp`).                     |
| `name`         | `string`   | **Yes**  | Display name for the core in the UI  (e.g., `PPSSPP`).                                |
| `default`      | `boolean`  |    No    | Set to `true` to mark this core as the primary emulator for the system.               |
| `achievements` | `boolean`  |    No    | Indicates support for RetroAchievements.                                              |
| `experimental` | `boolean`  |    No    | Marks the core as `experimental` or `in testing`.                                     |
| `extensions`   | `string[]` | **Yes**  | List of supported file extensions in **lowercase**   (e.g., `["cso", "iso", "pbp"]`). |

---

### Standalone Apps and Emulators

Independent apps and emulators that require a specific intent configuration to launch supported games and files.

```json
{
  "id": "ppsspp.default",
  "name": "PPSSPP Default",
  "provider": "PPSSPP",
  "default": true,
  "achievements": true,
  "extensions": ["cso", "iso", "pbp"],
  "intent": { ... }
}
```

| Field          | Type       | Required | Description                                                                                                                |
|:---------------|:-----------|:--------:|:---------------------------------------------------------------------------------------------------------------------------|
| `id`           | `string`   | **Yes**  | **Unique identifier** for this emulator on this platform (e.g., `ppsspp.default`, `gamehub.lite`).                         |
| `name`         | `string`   | **Yes**  | **Unique name** for this app or emulator configuration on this platform.                                                   |
| `provider`     | `string`   |    No    | Provider name used for grouping in UI if there are multiple variants (e.g., `PPSSPP` for `PPSSPP Default`, `PPSSPP Gold`). |
| `default`      | `boolean`  |    No    | Set to `true` to mark this config as the primary app or emulator for the system.                                           |
| `achievements` | `boolean`  |    No    | Indicates support for RetroAchievements or internal achievement systems.                                                   |
| `experimental` | `boolean`  |    No    | Marks the build as `experimental` or `in testing`.                                                                         |
| `extensions`   | `string[]` | **Yes**  | List of supported file extensions in **lowercase**   (e.g., `["cso", "iso", "pbp"]`).                                      |
| `intent`       | `object`   | **Yes**  | Android Intent configuration for the app or emulator. See [Intent Configuration](#intent-configuration).                   |

---

### Intent Configuration

Defines the technical parameters used to bridge the launcher and the app or emulator.

```json
{
  "packageName": "org.ppsspp.ppsspp",
  "className": "org.ppsspp.ppsspp.PpssppActivity",
  "action": "android.intent.action.VIEW",
  "category": "android.intent.category.DEFAULT",
  "data": "{FILE_URI}"
}
```

| Parameter     | Type       | Required | Description                                                                 |
|:--------------|:-----------|:--------:|:----------------------------------------------------------------------------|
| `packageName` | `string`   | **Yes**  | Android package ID (e.g., `org.ppsspp.ppsspp`).                             |
| `className`   | `string`   | **Yes**  | Specific Activity class name to (e.g., `org.ppsspp.ppsspp.PpssppActivity`). |
| `action`      | `string`   |    No    | Intent action. Defaults to `android.intent.action.MAIN`.                    |
| `category`    | `string`   |    No    | Intent category (e.g., `android.intent.category.DEFAULT`).                  |
| `data`        | `string`   |    No    | Data URI for the game file. **Should only contain** `{FILE_URI}`.           |
| `extra`       | `object`   |    No    | Key-value pairs of additional data (extras) to pass with the intent.        |
| `flags`       | `string[]` |    No    | Android Activity flags. See [Flags Behavior](#flags-behavior).              |

### Flags Behavior

Flags control how Android manages the emulator's activity and file permissions.

| Flag                        | Description                                                              |
|:----------------------------|:-------------------------------------------------------------------------|
| `ACTIVITY_CLEAR_TASK`       | Clears the target task before launching; starts with a fresh back stack. |
| `ACTIVITY_CLEAR_TOP`        | Destroys all activities above the target if it already exists.           |
| `ACTIVITY_SINGLE_TOP`       | Reuses the existing instance if it's already at the top of the stack.    |
| `ACTIVITY_NO_ANIMATION`     | Disables the system transition animation on launch.                      |
| `GRANT_READ_URI_PERMISSION` | Grants the app or emulator temporary read access to the provided file.   |

> **Flags Rules:**
> * **Omitted (null):** Uses default safety flags (`ACTIVITY_SINGLE_TOP`, `ACTIVITY_CLEAR_TOP`, `ACTIVITY_NO_ANIMATION`, `GRANT_READ_URI_PERMISSION`).
> * **Empty (`[]`):** No additional flags are applied.
> * **Populated:** Only the specified flags are used.

---

## 4. Placeholders

Placeholders are dynamic variables that the launcher replaces with actual file information at the moment of launch.

| Placeholder       | Description                                                   | Example                                                               |
|:------------------|:--------------------------------------------------------------|:----------------------------------------------------------------------|
| `{FILE_URI}`      | **Standard SAF content URI.** Mandatory for the `data` field. | `content://.../ROMs/psp/Grand Theft Auto.iso`                         |
| `{FILE_PATH}`     | Absolute physical path on the storage.                        | `/storage/emulated/0/ROMs/psp/Grand Theft Auto.iso`                   |
| `{FILE_NAME}`     | Filename without the extension.                               | `Grand Theft Auto`                                                    |
| `{FILE_NAME_EXT}` | Filename including the extension.                             | `Grand Theft Auto.iso`                                                |
| `{FILE_EXT}`      | File's extension suffix in lowercase.                         | `iso`                                                                 |
| `{FILE_EXT_DOT}`  | Extension including the leading dot.                          | `.iso`                                                                |
| `{FILE_MIME}`     | Identified MIME type of the file.                             | `application/octet-stream`                                            |
| `{FILE_SIZE}`     | Total file size in bytes (resolved as a **Number**).          | `123456789`                                                           |
| `{FILE_DIR_URI}`  | SAF URI of the parent directory.                              | `content://.../ROMs/psp/`                                             |
| `{FILE_DIR_PATH}` | Physical path of the parent directory.                        | `/storage/emulated/0/ROMs/psp/`                                       |
| `{FILE_DATA}`     | Parsed content (for JSON/XML/TXT)                             | `{"gameId": "123456789"}` \| `123456789` \| `"string value"` \| `etc` |
| `{FILE_DATA_RAW}` | Base64 encoded raw content                                    | `SGVsbG8g...d29ybGQ=`                                                 |

> **TIP:**
> If a field exactly matches a placeholder (e.g., `"size": "{FILE_SIZE}"`), the launcher preserves the original data type (Number, Boolean, or Object) instead of converting it to a string.

---

---

## Contributing Platforms, Apps, and Emulators

I welcome community contributions! To add a new system or update an emulator configuration, please submit a **Pull Request (PR)** with your changes to the `platforms` directory.

### Guidelines

* **File Naming:** Each JSON file in the `platforms` folder defines a single system. The filename must strictly match the `[Company] [Full Name].json` format (e.g., `Sony PlayStation Portable.json`).
* **Clean Syntax:** Always ensure valid JSON structure when appending new objects to configuration arrays.

---

### How to Add a New Platform

1. **Create a new file:** Inside the `platforms` directory, create a new `.json` file named after the hardware manufacturer and system name (e.g., `Sega Dreamcast.json`).
2. **Define the core structure:** Fill out the file using the baseline configuration format, including the basic system info, target scan folders, and at least one compatible app or emulator.

```json
{
  "id": "short-id",
  "name": "Short Name",
  "fullName": "Full System Name",
  "details": {
    "company": "Manufacturer",
    "release": "YYYY-MM-DD",
    "description": "A brief sentence about the system's legacy",
    "type": "console"
  },
  "folders": ["folder_name"],
  "emulators": {
    "retroarch": [],
    "standalone": []
  }
}
```

3. **Submit your PR:** Commit your new file to a new branch (e.g., `platform/new/sega-dreamcast`) and open a Pull Request with a descriptive title (e.g., `[New Platform] Sega Dreamcast`).

---

### How to Add a New Emulator

1. **Locate the target platform:** Open the corresponding JSON file in the `platforms` directory (e.g., `Microsoft Windows.json`).
2. **Append your configuration:** Insert your new emulator block at the end of the `retroarch` or `standalone` array.
3. **Verify the JSON structure:** Remember to add a comma separating the previous emulator object from your new entry.

```json
"emulators": {
  "standalone": [
    { "existing_emulator_config..." }, 
    {
      "id": "gamehub.lite",
      "name": "GameHub Lite",
      "provider": "GameHub",
      "extensions": ["localgameid", "steamappid"],
      "intent": {
        "packageName": "gamehub.lite",
        "className": "com.xj.landscape.launcher.ui.gamedetail.GameDetailActivity",
        "action": "gamehub.lite.LAUNCH_GAME",
        "extra": {
          "autoStartGame": true,
          "localGameId": "{FILE_DATA}",
          "steamAppId": "{FILE_DATA}"
        }
      }
    }
  ]
}

```

4. **Submit your PR:** Commit the changes to a new branch (e.g., `emulator/new/microsoft-windows--gamehub-lite`) and open a Pull Request. Provide a concise title and a brief description of the emulator or app you are adding (e.g., `[New Emulator] Microsoft Windows -- GameHub Lite`)

---

### How to Update an Existing App or Emulator

1. **Locate the target platform:** Open the corresponding JSON file in the `platforms` directory (e.g., `Microsoft Windows.json`).
2. **Modify the configuration:** Find the specific emulator object inside the `retroarch` or `standalone` array and update its parameters.
3. **Submit your PR:** Commit the changes to a new branch (e.g., `emulator/update/microsoft-windows--gamehub-lite--updated-launch-parameters`) and open a Pull Request. Provide a concise title and a brief description of what was changed and why (e.g., `[Update Emulator] Microsoft Windows -- GameHub Lite -- Updated Launch Parameters`).
