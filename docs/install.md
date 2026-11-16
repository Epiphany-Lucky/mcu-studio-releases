# MCU Studio 安装与更新

## Windows

下载 `MCU-Studio-0.1.0-Setup-x64.exe` 后运行安装向导。默认采用当前用户安装，可选择独立安装位置。开始菜单或桌面快捷方式启动原生工作台。

桌面壳使用系统 Edge WebView2 Runtime。随软件提供固件执行后端和相关运行资源。便携 ZIP 完整解压后运行 `MCU-Studio.exe`。HTML 本地包使用 `Open-Web.cmd` 启动本地服务和浏览器。

新版安装器沿用本产品的安装标识和注册位置。卸载入口位于 Windows 已安装应用。用户自行导出的工程和固件按原位置保管。

## Ubuntu 与 Debian

下载同版 amd64 DEB，在下载目录运行下面的命令。

```bash
sudo apt install ./mcu-studio_0.1.0_amd64.deb
```

从应用菜单打开 MCU Studio，工作台显示在系统默认浏览器。系统需要可用的图形浏览器及 DEB 声明的运行依赖。Linux 便携版本完整解压后运行目录中的 `./mcu-studio`，保留符号链接和执行权限。

卸载使用下面的命令。

```bash
sudo apt remove mcu-studio
```

## 应用内更新

更新源为 [MCU Studio 官方发行页](https://github.com/Epiphany-Lucky/mcu-studio-releases/releases)。软件下载后校验大小和 SHA-256，再交接安装。Windows 使用本产品当前用户安装器。Linux DEB 安装过程使用系统授权窗口，便携版本通过已校验压缩包更新。

安装和会话退出行为分别在同版验收记录中登记。每次发布保留确定提交、安装摘要和公告。新版本通过验证后清理旧安装分发附件，工程文件继续由用户保管。

## 运行资源与许可

运行包包含桌面壳或启动器、本地服务、Renode 后端、芯片配置、外设适配、Web 界面、实验固件及许可资料。第三方软件采用其各自许可，相关声明与对应源码资料见 `THIRD_PARTY_NOTICES.md`。
