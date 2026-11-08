DLSS Neural Rendering Unlock - Portable v1.0
=====================================================

WHAT IT DOES
    Unlocks the in-game "Neural Rendering" option (NVIDIA NGX feature 18)
    on RTX 40-series (Ada) cards, where it is normally greyed out.
    The toggle itself is the game's own official NGX pipeline - this pack only
    removes the hardware gate. Tested on RTX 4070 + Hogwarts Legacy.

    It does NOT work on RTX 20/30 series: the proxy only rewrites the Ada
    architecture id (0x190 -> 0x1B0), and the patched runtime is an Ada build.

REQUIREMENTS
    - Windows 10/11 x64
    - NVIDIA RTX 40-series GPU
    - A game whose graphics settings already contain a Neural Rendering entry
    - Game closed before installing

QUICK START (one click)
    1. Close the game.
    2. Double-click 1-Install.cmd.
       - If this folder sits inside the game's executable directory, the target
         folder is detected automatically.
       - Otherwise a folder picker appears; select the folder that contains the
         game exe (e.g. ...\HogwartsLegacy\Phoenix\Binaries\Win64).
       - You can also drag the game folder onto 1-Install.cmd.
    3. Wait for "Install complete".
    4. In game: Display/Graphics -> Upscaling = DLSS Quality (or higher),
       then set Neural Rendering to On.

UNINSTALL
    Double-click 2-Uninstall.cmd.
    Any file that was overwritten is backed up first to
    <game folder>\_NR_backup_<timestamp>\ and restored on uninstall.

WHAT GETS INSTALLED (into the game's exe folder)
    dinput8.dll                  GPU spoof proxy. Reports the GPU as Blackwell
                                 (arch 0x1B0) and fakes the PCI id of an
                                 RTX 5090 (0x2B8510DE).
    winmm.dll + 033-loader.ini   Generic injector. Loads the proxy into the
                                 process named in 033-loader.ini (Targets=).
    nvngx_dlssnr.dll             Patched Neural Rendering runtime (310.8.0).
                                 The untouched official 310.9.16 refuses to
                                 create the feature on Ada (0xBAD00002) -
                                 that patched build is what actually runs.
    MetroNRFeature18Probe.on     Feature switches read by the proxy.
    MetroNRDirectSnippet.on
    .nr-unlock.json              Install record used by the uninstaller.

HOW IT WORKS (one line)
    Proxy makes NGX believe this is Blackwell + RTX 5090, so the capability
    query passes and the toggle becomes available; the patched runtime lets the
    feature actually be created and evaluated on this GPU.

NOTES / CAVEATS
    - Only useful for games that already ship a Neural Rendering setting.
    - Conflicts possible with ReShade or other dinput8 / winmm proxies.
      Disable those first if the game crashes.
    - The proxy hooks NVAPI and DXGI. A few games may crash; uninstall restores
      everything.
    - Fully offline. Nothing is uploaded. All files stay on this machine.
    - The untouched official nvngx_dlssnr.dll under
      Engine\Plugins\Runtime\Nvidia\StreamlineCore\... is left as-is.
