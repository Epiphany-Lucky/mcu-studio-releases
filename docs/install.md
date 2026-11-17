# MCU Studio 安装与更新

## Windows

下载单文件 `MCU-Studio-0.2.1-Setup-x64.msi` 后双击运行。安装资源内嵌于 MSI，采用系统级安装，需要在 Windows 用户账户控制窗口中提供管理员授权，可选择包含中文和空格的独立安装位置。安装完成后从开始菜单或桌面快捷方式启动原生工作台。

Windows Installer 管理系统 MSI 的文件组件、修复、升级和卸载。安装同系列新版 MSI 会更新已有安装，旧版安装包会阻止覆盖较新版本。维护操作按 Windows 提示授权，卸载入口位于 Windows 已安装应用。

已有 0.1.0 EXE 安装时，可通过 EXE 更新通道升级到 0.2.1。选择迁移至系统 MSI 时，需要管理员授权。迁移在移除旧安装前准备经过 SHA-256 核对的恢复安装器和恢复记录，再执行旧安装移除与 MSI 安装。发生迁移失败时按恢复记录还原旧安装与工程。迁移期间保留安装向导和恢复提示，完成后核对版本。

发行页同时提供 `MCU-Studio-0.2.1-Setup-x64.exe`，采用当前用户安装，已有 EXE 安装继续通过其更新通道升级。

桌面壳使用系统 Edge WebView2 Runtime。随软件提供固件执行后端和相关运行资源。便携 ZIP 完整解压后运行 `MCU-Studio.exe`。HTML 本地包使用 `Open-Web.cmd` 启动本地服务和浏览器。

MSI 使用本产品独立的安装系列标识，记录安装技术、实际路径和产品代码，供后续更新选择对应安装包。用户自行导出的工程和固件按原位置保管。

## Ubuntu 与 Debian

下载同版 amd64 DEB，在下载目录运行下面的命令。

```bash
sudo apt install ./mcu-studio_0.2.1_amd64.deb
```

从应用菜单打开 MCU Studio，工作台显示在系统默认浏览器。系统需要可用的图形浏览器及 DEB 声明的运行依赖。Linux 便携版本完整解压后运行目录中的 `./mcu-studio`，保留符号链接和执行权限。

卸载使用下面的命令。

```bash
sudo apt remove mcu-studio
```

## 应用内更新

更新源为 [MCU Studio 官方发行页](https://github.com/Epiphany-Lucky/mcu-studio-releases/releases)。软件根据当前安装技术自动选包，下载后校验大小和 SHA-256，再交接安装。系统 MSI 更新显示“授权安装并重启”，安装阶段由 Windows 请求管理员授权并运行 `msiexec`。取消授权后保留已校验安装包，并重新启动当前应用，方便稍后重试。EXE 安装沿用当前用户的 EXE 安装器。Linux DEB 安装过程使用系统授权窗口，便携版本通过已校验压缩包更新。

安装和会话退出行为分别在同版验收记录中登记。每次发布保留确定提交、安装摘要和公告。新版本通过验证后清理旧安装分发附件，工程文件继续由用户保管。

## 运行资源与许可

运行包包含桌面壳或启动器、本地服务、Renode 后端、芯片配置、LQFP144 引脚定义、时钟与定时器适配、Web 界面、28 项实验菜单及许可资料。完整解压运行包，保留 `chips/`、`platforms/`、`peripherals/`、`runtime/`、`firmware/` 和 `web/` 的目录结构。

软件启动后，可在引脚表搜索 1–144 的物理脚号，在时钟树查看 SYSCLK/HCLK/PCLK，在定时器面板选择 TIM1–TIM14。功能路径与操作见 [快速开始](quick-start.md)。

第三方软件采用其各自许可，相关声明与对应源码资料见 `THIRD_PARTY_NOTICES.md`。
