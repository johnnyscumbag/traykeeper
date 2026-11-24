# TrayKeeper

A Windhawk mod for preserving Windows 11 notification-area icon order while still allowing user-controlled reordering.

TrayKeeper is a fork of [Tray Order Lock](https://github.com/Yusseter/tray-order-lock) by **Yusseter**. The original project provided the foundation for TrayKeeper's notification-area ordering, logical icon identification, persistence, and restoration work. The upstream project and its author are credited here under the original MIT license.

## Status

TrayKeeper is currently an experimental fork under active development.

The current code is based on Tray Order Lock 0.2.0 and retains its existing ordering functionality while the fork investigates improvements for newer Windows 11 builds.

The long-term goal is to make TrayKeeper preserve the complete user-defined state of notification-area icons, including both their ordering and their visible/overflow placement.

Visibility persistence is **not implemented yet**. It is an area of planned investigation and development.

The mod is not yet published in Windhawk's official mod catalog.

## Current features

TrayKeeper currently provides the ordering functionality inherited from Tray Order Lock.

### Lock all reordering

Preserves the original Tray Order Lock 0.1.0 behavior.

- Blocks notification-area icon move requests.
- Prevents manual tray reordering while the mode is active.
- Does not directly modify UIOrderList.

### Preserve order, allow manual changes

Allows the user to rearrange tray icons while protecting the resulting order from later application, Windows or Explorer-driven recreation.

- Learns successful manual tray moves.
- Stores a canonical logical order in Windhawk local storage.
- Restores known icons to their canonical relative position when necessary.
- Preserves the canonical order across complete Explorer process restarts.
- Uses Windows' own notification-area move path for restoration.
- Does not directly write or reconstruct UIOrderList.

Logical icon identity currently uses:

- IconGuid when available.
- Otherwise, a version-normalized executable path plus UID.

Ambiguous or unsupported identities are deliberately left untouched.

## New icon handling

In **Preserve order, allow manual changes** mode, genuinely new icons can use one of two policies:

- **Use Windows default position**
- **Place new icons at the end**

The behavior of **Place new icons at the end** is currently being investigated on newer Windows 11 builds. The upstream 0.2.0 validation environment was Windows 11 25H2, build 26200.8973.

## Planned development

The main direction for TrayKeeper is to extend the existing ordering architecture into persistent tray-state management.

The intended behavior is:

- Remember each icon's position.
- Remember whether each icon belongs in the visible tray or the Windows overflow area.
- Treat genuinely new icons as overflow by default.
- Learn manual visibility changes made by the user.
- Restore the saved visibility state when an application's tray icon is recreated after an update.
- Continue allowing manual reordering and remember those changes.
- Preserve the state across Explorer restarts and application updates.
- Avoid replacing the normal Windows notification-area UI or creating custom tray folders.

The goal is for TrayKeeper to remember the tray **as the user arranged it**, rather than imposing a fixed layout.

## Requirements

- Windows 11
- 64-bit Explorer
- Windhawk

The current ordering implementation relies on internal Windows taskbar symbols and interfaces, which can change between Windows builds.

The upstream 0.2.0 runtime validation was performed on Windows 11 25H2, build 26200.8973.

## Installation

Until TrayKeeper is submitted to the official Windhawk catalog:

1. Open the repository's latest GitHub Release.
2. Download the Windhawk source file.
3. Open Windhawk.
4. Create a new local mod.
5. Replace the local mod source with the downloaded source.
6. Select **Compile Mod**.
7. Enable the mod.
8. Choose the desired ordering behavior in Settings.

## Settings

### Ordering behavior

- **Lock all reordering**
- **Preserve order, allow manual changes**

### New icon placement

Used with Preserve order mode:

- **Use Windows default position**
- **Place new icons at the end**

## Validation

The inherited Tray Order Lock 0.2.0 implementation was validated for:

- Manual moves being learned and persisted.
- Known logical icons being restored after recreation.
- Canonical ordering surviving Explorer restarts.
- Automatic restore moves not being learned back as manual changes.
- Restore operations being suppressed safely during user-initiated moves.
- Both new-icon placement policies.
- The **Lock all reordering** regression.
- Explorer stability during the final validated tests.

TrayKeeper's ongoing development is focused on reproducing and improving this behavior on current Windows builds before adding the planned visibility persistence.

See [docs/research.md](docs/research.md) for the investigation and technical validation history.

## Repository layout

```text
tray-order-lock.wh.cpp
README.md
LICENSE
docs/
└── research.md
research/
├── analyzers/
│   ├── system-tray-index-analyzer.wh.cpp
│   ├── taskbar-symbol-enumerator.wh.cpp
│   ├── tray-add-path-analyzer.wh.cpp
│   └── tray-order-lock-analyzer.wh.cpp
└── experiments/
    ├── chatgpt-stable-tray-id-test.wh.cpp
    ├── nvidia-stable-tray-id-test.wh.cpp
    └── stable-tray-icons.wh.cpp
scripts/
└── copy-mod-to-clipboard.ps1
```

The repository root intentionally contains only the actual Windhawk source. Analyzer and experimental mods are retained under research/ for development history and reproducibility.

## VS Code workflow

The repository files are the source of truth.

Press Ctrl+Shift+B while a .wh.cpp file is active to run the included **Copy active Windhawk mod to clipboard** task.

The task copies the complete active source file to the clipboard. Windhawk remains responsible for **Compile Mod**, mod enablement and runtime logging.

## Attribution

TrayKeeper is based on **Tray Order Lock** by **Yusseter**:

- Upstream repository: https://github.com/Yusseter/tray-order-lock
- Author: https://github.com/Yusseter
- License: MIT

The original copyright notice and MIT license are retained in this repository.

TrayKeeper is an independent fork and is not affiliated with or endorsed by the original author.

## License

TrayKeeper is released under the MIT License. See [LICENSE](LICENSE) for the full license text and original copyright notice.