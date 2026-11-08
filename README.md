DLSS 神经渲染解锁 整合包 v1.0
=====================================================

作用
    让 RTX 40 系（Ada）在“游戏内本来就有神经渲染选项”的游戏里，把原本
    灰色的开关点亮并真正运行。已在 Hogwarts Legacy 上实测通过。

用法（一键）
    1. 关掉游戏。
    2. 双击 1-Install.cmd。
       · 整合包放在游戏主程序所在目录里，它会自己认；
       · 放在别处也行，会弹窗让你选目录（例如
         ...\HogwartsLegacy\Phoenix\Binaries\Win64）。
       也可以把游戏目录直接拖到 1-Install.cmd 上。
    3. 看到“安装完成”就好了。
    4. 进游戏：画面设置 → 上调模式选 DLSS 品质 → 神经渲染 开。

卸载
    双击 2-Uninstall.cmd。安装前被覆盖的同名文件会自动备份在
    游戏目录\_NR_backup_时间戳\，卸载时会自动还原。

装了什么
    dinput8.dll                 GPU 伪装代理：把 Ada 报成 Blackwell，
                                并把 PCI ID 伪造成 RTX 5090
    winmm.dll + 033-loader.ini  通用注入器：按进程名把上面那个代理拉进游戏
    nvngx_dlssnr.dll            打过补丁的神经渲染运行时（官方版在 40 系上
                                会直接拒绝创建，这一份才能真跑起来）
    MetroNRFeature18Probe.on    两个开关文件
    MetroNRDirectSnippet.on

注意
    · 只对“设置里本来就有神经渲染这一项”的游戏有效，没有这项的游戏装了也没用。
    · 和 ReShade、其他 dinput8 / winmm 代理可能抢 hook。冲突时先停用它们。
    · 代理会 hook NVAPI 与 DXGI，个别游戏可能崩溃；卸载即可还原。
    · 全程离线，不联网。
