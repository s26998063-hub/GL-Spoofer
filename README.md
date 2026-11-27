<div align="center">

# GL Spoofer (A36 Good Lock Spoofer)

**LSPosed module · Samsung Galaxy · Good Lock access**

[![Android](https://img.shields.io/badge/Android-13%2B%20(unverified)-3DDC84?logo=android&logoColor=white)](#compatibility)
[![Framework](https://img.shields.io/badge/Framework-LSPosed-5865F2)](#requirements)
[![License](https://img.shields.io/badge/License-see%20repository-lightgrey)](#)

**English** · [Русский](README_RU.md)

</div>

---

## Overview

**A36 Good Lock Spoofer** is an LSPosed module that makes **Good Lock** and **Galaxy Store** see a Samsung Galaxy A36 identity (`SM-A366B`, CSC `EUX`) in order to make Good Lock and its modules available on Samsung devices where the catalogue may otherwise be restricted.

The spoof is applied inside the two targeted apps. It does not intentionally change the device identity seen by unrelated apps or the system interface.

> **This is an app-compatibility workaround, not an official Samsung feature.** Availability of Good Lock modules still depends on Samsung's services, region, app versions and server-side checks. The module cannot guarantee that every module will install or work.

## Compatibility

| Android / One UI | Status |
|---|---|
| **Android 17 · One UI 9.0** | <strong>Confirmed working</strong> on the maintainer's device |
| Android 16 · One UI 8.5 | Unverified — may work, but is not confirmed |
| Android 13 · One UI 6.1 and intermediate versions up to One UI 8.5 | Unverified — may work on some configurations; no guarantee |

Compatibility is not determined by the Android version alone. Root method, Zygisk implementation, LSPosed fork, Samsung firmware, Good Lock and Galaxy Store versions can all affect the result.

**Please report your Android version, One UI version, device model and whether it works** when submitting feedback. Do not describe an untested version as supported.

## What it changes

- Spoofs selected build identity fields to **Galaxy A36 5G (`SM-A366B`)**.
- Spoofs the CSC / sales region as **EUX** and related product-code values.
- Hooks selected Samsung floating-feature checks in the targeted apps.
- Returns German SIM/network operator information to the targeted apps.
- Targets only these package names:
  - `com.samsung.android.goodlock`
  - `com.sec.android.app.samsungapps`
- Runs a one-time Good Lock data cleanup on its first hooked launch, described below.

### Important: one-time Good Lock data reset

On the first Good Lock launch after the module's hook is active, the module attempts to delete Good Lock's local:

`shared_prefs`, `databases`, `cache`, `code_cache`, `files`, and `no_backup` directories.

This is intended to remove stale local state so Good Lock can rebuild its catalogue using the spoofed identity. **It can reset Good Lock preferences and locally stored data.** The operation is marked as completed after the first attempt; normal subsequent launches and reboots do not repeat it.

If you do not want Good Lock's local data cleared, do not enable/use this build. Back up anything important where possible before proceeding.

## Requirements

You need all of the following:

1. A Samsung device with **bootloader/root access** already set up.
2. **Magisk** or **KernelSU**.
3. A working Zygisk implementation, such as [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext/releases), installed using the instructions for your root manager.
4. **LSPosed**. This project documents the [LSPosed Vector fork by JingMatrix](https://github.com/JingMatrix/LSPosed/releases) because support for newer Android versions depends on the framework build you use.
5. The A36 Good Lock Spoofer APK from this repository's [Releases](../../releases).

> Rooting, installing modules and changing system-level components carry risks. Use builds intended for your Android version, keep a recovery/backup plan, and follow the official instructions for your root manager and LSPosed fork. This project does not provide root access.

## Installation

Follow the steps in order. Menu names can differ slightly between KernelSU, Magisk and LSPosed versions.

### 1. Set up Zygisk and LSPosed

1. Install and configure Magisk or KernelSU on your device.
2. Install a compatible Zygisk implementation. For Zygisk Next, use its [official Releases page](https://github.com/Dr-TSNG/ZygiskNext/releases) and follow its current installation instructions.
3. Install a compatible LSPosed build. The [JingMatrix LSPosed Vector releases](https://github.com/JingMatrix/LSPosed/releases) are one option; check the release notes for your Android version.
4. Reboot when the framework's instructions ask you to.

Do not continue until LSPosed is running and its manager can be opened.

### 2. Install the spoofer APK

1. Open this repository's [latest release](../../releases/latest).
2. Download the APK asset for **A36 Good Lock Spoofer**.
3. Install the APK on your phone.

Only install APKs from a source you trust.

### 3. Enable the module for Galaxy Store

1. Open **LSPosed Manager**.
2. Go to **Modules** and select **A36 Good Lock Spoofer**.
3. Enable the module.
4. Open its **Scope** page and verify that **Galaxy Store** (`com.sec.android.app.samsungapps`) is selected.
5. Perform a **soft reboot** if your root manager/framework provides that option. Otherwise, use a normal reboot.

The module's declared default scope includes Galaxy Store. Still verify the actual scope shown by your LSPosed Manager.

### 4. Install Good Lock

1. Open **Galaxy Store**.
2. Search for **Good Lock**.
3. Install the official Samsung app if it is available.
4. If it does not appear, reopen Galaxy Store after confirming the module is enabled and its scope includes Galaxy Store.

### 5. Add Good Lock to the LSPosed scope

1. Return to **LSPosed Manager → Modules → A36 Good Lock Spoofer**.
2. Open **Scope**.
3. Select **Good Lock** (`com.samsung.android.goodlock`).
4. Confirm that both **Good Lock** and **Galaxy Store** are selected.
5. Reboot the device (or perform a supported soft reboot).

### 6. KernelSU-specific setting

If you use **KernelSU**:

1. Open **KernelSU → Superuser**.
2. Enable **Show system apps**, if the option is available.
3. Find **Good Lock** and **Galaxy Store**.
4. For both apps, make sure **Unmount modules** is **disabled**.

The exact wording and location of this option may vary by KernelSU version. Do not change unrelated app settings.

### 7. Open Good Lock

Launch Good Lock and check whether its module catalogue is available. Install modules from Galaxy Store as usual.

## Troubleshooting

| Problem | What to check |
|---|---|
| Good Lock is missing from Galaxy Store | Confirm the Galaxy Store scope is enabled, the module is active, and the framework is running. Reopen Galaxy Store after a reboot. |
| Good Lock opens but the catalogue is empty | Confirm Good Lock is also selected in LSPosed scope. The one-time cleanup runs only on the first hooked Good Lock launch; it is not a repeated cache-clearing tool. |
| Module is enabled but has no effect | Check both package scopes, framework status, root-manager settings and whether the LSPosed build supports your Android version. |
| It works on one firmware but not another | Compatibility may depend on Samsung firmware and app versions. Report the exact Android, One UI and device versions. |
| A Good Lock module is unavailable or fails | The spoofer does not guarantee server-side availability or compatibility of individual Good Lock plugins. |

For useful bug reports, include:
- Device model
- Android version and One UI version
- Root manager and version
- Zygisk implementation and version
- LSPosed fork/build
- Good Lock and Galaxy Store versions
- A short description of the issue and relevant logs (remove personal information)

## Disable or uninstall

1. Open LSPosed Manager → Modules.
2. Disable **A36 Good Lock Spoofer**.
3. Reboot the device.
4. If you want to remove it completely, uninstall its APK after disabling it.

Disabling the module stops its hooks from being applied to future app processes. It does not restore Good Lock data deleted during the one-time cleanup.

## Links

- [Latest APK release](../../releases/latest)
- [All releases](../../releases)
- [Zygisk Next](https://github.com/Dr-TSNG/ZygiskNext)
- [LSPosed Vector by JingMatrix](https://github.com/JingMatrix/LSPosed)
- [Developer: @AstrodiR](https://t.me/AstrodiR)

## Credits

Created and maintained by **[@AstrodiR](https://t.me/AstrodiR)**.

---

<sub>Unofficial community project. Samsung, Galaxy, Galaxy Store and Good Lock are trademarks of their respective owners. This project is not affiliated with or endorsed by Samsung.</sub>
