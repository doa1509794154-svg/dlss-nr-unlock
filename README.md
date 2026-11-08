# DLSS Neural Rendering Unlock - Portable v1.0

> **Disclaimer / 免责声明**
> This project is published for **technical research and educational sharing only**.
> It distributes no game files, no game assets and no licensed content — only small
> helper scripts and a proxy that removes a hardware gate at runtime.
> It is not affiliated with, endorsed by, or supported by NVIDIA, any game publisher,
> or any other party. All trademarks belong to their owners.
> Use at your own risk: back up your files first, and note that modifying a game's
> binaries may violate its EULA or local law in some regions. The author accepts no
> liability for bans, crashes, data loss or any other damage.
>
> 本项目仅用于技术研究与分享，不提供任何破解内容，与 NVIDIA 及游戏厂商无关，
> 使用者需自行承担风险。

## What it does

Unlocks the in-game "Neural Rendering" option (NVIDIA NGX feature 18) on RTX 40-series
(Ada) cards, where it is normally greyed out. The toggle itself is the game's own
official NGX pipeline — this pack only removes the hardware gate.
Tested on RTX 4070 + Hogwarts Legacy.

It does **not** work on RTX 20/30 series: the proxy only rewrites the Ada architecture
id (0x190 → 0x1B0), and the patched runtime is an Ada build.

## Requirements

- Windows 10/11 x64
- NVIDIA RTX 40-series GPU
- A game whose graphics settings already contain a Neural Rendering entry
- Game closed before installing

## Quick start (one click)

1. Close the game.
2. Double-click `1-Install.cmd`.
   - If this folder sits inside the game's executable directory, the target folder is
     detected automatically.
   - Otherwise a folder picker appears; select the folder that contains the game exe
     (e.g. `...\HogwartsLegacy\Phoenix\Binaries\Win64`).
   - You can also drag the game folder onto `1-Install.cmd`.
3. Wait for "Install complete".
4. In game: Display/Graphics → Upscaling = DLSS Quality (or higher), then set
   Neural Rendering to On.

## Uninstall

Double-click `2-Uninstall.cmd`. Any file that was overwritten is backed up first to
`<game folder>\_NR_backup_<timestamp>\` and restored on uninstall.

## What gets installed (into the game's exe folder)

| File | Purpose |
| --- | --- |
| `dinput8.dll` | GPU spoof proxy. Reports the GPU as Blackwell (arch 0x1B0) and fakes the PCI id of an RTX 5090 (0x2B8510DE). |
| `winmm.dll` | Generic injector. It loads the proxy into the game process. |
| `nvngx_dlssnr.dll` | Patched Neural Rendering runtime (310.8.0). The untouched official 310.9.16 refuses to create the feature on Ada (0xBAD00002) — this patched build is what actually runs. |
| `MetroNRFeature18Probe.on`, `MetroNRDirectSnippet.on` | Feature switches read by the proxy; written by the installer. |
| `.nr-unlock.json` | Install record used by the uninstaller. |

## How it works (one line)

The proxy makes NGX believe this is Blackwell + RTX 5090, so the capability query
passes and the toggle becomes available; the patched runtime lets the feature actually
be created and evaluated on this GPU.

## Notes / caveats

- Only useful for games that already ship a Neural Rendering setting.
- Conflicts are possible with ReShade or other dinput8 / winmm proxies. Disable those
  first if the game crashes.
- The proxy hooks NVAPI and DXGI. A few games may crash; uninstall restores everything.
- Fully offline. Nothing is uploaded. All files stay on this machine.
- The untouched official `nvngx_dlssnr.dll` under
  `Engine\Plugins\Runtime\Nvidia\StreamlineCore\...` is left as-is.
