# MCU Studio 0.2.0 验证记录

记录日期为2026年11月17日。本记录覆盖STM32F407功能级固件仿真、Windows／Linux发行包、系统MSI安装与更新交接。

## 正式构建来源

发行二进制由源码提交 `02f3b717dda50eb674a162f6c47369ca1cd9ca0f` 构建。选定CI为 `44009932680`，运行时间为2026年11月17日03:24:03至03:59:41 UTC，全部六项任务成功。CI链接与原始日志受私有源码仓库的访问权限控制。[完整CI记录](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680)

| CI任务 | 结果 | 原始记录 |
|---|---|---|
| Windows源码验证 | 成功 | [Windows任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680/job/133794238378) |
| Ubuntu 22.04源码验证 | 成功 | [Linux任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680/job/133794238412) |
| Windows安装器独立验证 | 成功 | [安装器任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680/job/133794237999) |
| Windows发行包与完整安装验证 | 成功 | [Windows打包任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680/job/133798088541) |
| Linux发行包与安装验证 | 成功 | [Linux打包任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680/job/133798088418) |
| 第三方对应源码归档 | 成功 | [第三方源码任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44009932680/job/133798088403) |

## 测试结果

| 环境与入口 | 实际结果 |
|---|---|
| Windows源码测试 | 发现122项，通过112项，跳过10项Linux专用用例，零失败，123.963秒 |
| Linux源码测试 | 发现122项，通过121项，跳过1项Windows PowerShell执行用例，零失败，118.383秒 |
| Windows／Linux源码服务 | 各16项固件执行流程、26项浏览器验收通过 |
| Windows冻结发行包 | 16项固件执行流程、26项浏览器验收通过 |
| Linux实际安装的DEB | 16项固件执行流程、26项浏览器验收通过，启动器复用与正常退出通过 |
| Windows实际安装的EXE与MSI | 各完成GPIO、CH340、OLED、LCD四项ARM固件执行与工程导出／恢复往返 |

固件执行结果来自Renode后端运行编译后的ARM映像，包含真实指令执行、串口返回和显示像素断言。浏览器验收覆盖器件自由组合、接线与工程恢复、主题切换，以及已有串口、显示、PLL和定时器操作。

## 系统MSI与迁移

正式MSI采用 `perMachine` x64系统安装，内嵌CAB，默认安装到64位Program Files，支持自选目录。安装、修复和更新需要Windows管理员授权。安装标记使用HKLM64，记录安装技术、目录、ProductCode、版本及 `Scope=machine`，公共开始菜单和桌面快捷方式通过实际检查。EXE保留当前用户安装与更新通道。

管理员CI使用随机独立测试身份，完成新装、丢失文件修复、主要升级、降级拦截、同目录EXE覆盖拦截、卸载及用户工程文件保留。Windows Installer实际安装上下文为4，即机器范围。

MSI主要升级进行了三个失败时点的真实回滚验证。三个时点均先执行旧文件删除，再触发失败。发布后探针还直接记录旧产品状态为 `-1`、新产品状态为 `5`。回滚完成后，旧产品恢复为已安装状态 `5`，新产品查询为 `-1`，旧版文件字节和登记恢复，新版独有文件消除，工程文件保留。

Inno迁移在移除旧版前核验恢复安装包的长度和SHA-256并备份工程。恢复包校验失败时原安装保留。文件写入后、登记写入后及产品发布后三个失败时点均实际恢复原Inno版本、目录、登记和工程。正常迁移完成后保留单一有效安装登记。默认系统目录迁移与显式选择原目录的迁移均通过，旧目录内的工程按原路径保留。

独立安装器任务与完整发行包任务分别重复执行上述生命周期检查。完整0.2.0 MSI还实际安装到中文空格路径，并完成四项ARM固件执行与工程往返。两轮结果均为成功，清理错误与迁移日志收集错误列表均为空。

## 实际更新helper

EXE与系统MSI更新helper均实际执行了安装器SHA-256校验、等待应用退出、中文空格目标目录安装、安装退出码检查及应用重启见证。系统MSI记录请求管理员权限，实际Windows Installer返回0，HKLM五项标记正确，最后使用独立ProductCode正常卸载测试产品。此项使用独立测试版本9.0.0，生产目录的0.1.0至0.2.0升级状态见末节。

## 发行附件摘要

以下七项附件已上传发行草稿。GitHub返回的附件digest与本地SHA-256逐项一致，`SHA256SUMS-0.2.0.txt`已上传。清单依据归档文件 `artifacts/ci-44009932680/release-inventory.json`。

| 文件 | 字节数 | SHA-256 |
|---|---:|---|
| `MCU-Studio-0.2.0-Setup-x64.msi` | 218151059 | `8c0904032c3b0e3b3c1bccced3ddc530e47eea9b8bbdf07b43a26eb6e96fd639` |
| `MCU-Studio-0.2.0-Setup-x64.exe` | 127697373 | `9c7ea5f9b71cb0010e01ebd29b0c11be20a77ffe50295fd520b5c8eb9a462dd0` |
| `MCU-Studio-0.2.0-win-x64.zip` | 181653237 | `758e545212c973311e3bcd91131c4ede76c085980047fed7bb6c259dda6dc111` |
| `MCU-Studio-0.2.0-html-local.zip` | 120305651 | `3b84386376bbc8e3bcbeecd759c07aa5e9c027785db8433b3a6b68923d65a008` |
| `mcu-studio_0.2.0_amd64.deb` | 66965568 | `818a2c2c285d62172859e98501258eee2e6f404732f7585c77f339748dd5c859` |
| `MCU-Studio-0.2.0-linux-x64.tar.gz` | 87570181 | `161094888fbb4c113197e44e58c93a075e9e672dd1b8c1ce0ab85756d631cfab` |
| `renode_1.17.0_source.tar.xz` | 329418672 | `65170aa79966044704ddef274568ed8b6e78b9f08edde578de33befee28a1eee` |

## 证据归档

正式CI的Windows便携包在本机原生WPF入口再次通过。120 DPI下窗口为1600×980并完整位于1920×1020工作区，LCD实际写入76800次并输出4种颜色，浏览器错误为0，关闭后退出码为0且后端停止。内容截图及打包CSS核对确认白色卡片、近黑文字和加深边框。桌面整屏截图受到另一窗口遮挡，外观依据本实例内容截图记录。证据为`artifacts/native-ci-v0.2.json`、`native-ci-v0.2-content.png`与`native-ci-v0.2-colors.json`。

以下相对路径以验收工作区为根。CI原始日志及JSON／截图归档保留对应测试身份与执行环境。

- Windows与Linux源码测试原始日志分别为 `artifacts/ci-44009932680/job-133794238378.log`、`job-133794238412.log`。
- 源服务固件与浏览器结果位于 `artifacts/ci-44009932680/source-proof/Windows/` 和 `source-proof/Linux/`，分别包含 `source-firmware-*.json` 与 `source-browser-*/result.json`。
- Windows冻结16项固件与26项浏览器结果为 `artifacts/ci-44009932680/windows-install-verification/frozen-windows-firmware.json` 及 `frozen-windows-browser/result.json`。
- 完整MSI生命周期与实际安装后固件结果为 `artifacts/ci-44009932680/windows-install-verification/msi-smoke-Msi4b239b938d6c/result.json`，同目录包含三个主要升级回滚日志、三个Inno恢复日志及按测试身份收集的迁移helper证据。
- 独立系统MSI预验证结果为 `artifacts/ci-44009932680/installer-preflight/msi-smoke-Msi5bfc262df9bf/result.json`。实际更新helper记录为同归档内的 `self-update-helper-smoke.json`。
- Linux已安装程序的结果位于 `artifacts/ci-44009932680/linux-install-proof/`，包含 `frozen-linux-firmware.json`、`frozen-linux-browser/result.json` 与 `linux-launcher-verification.json`。

## 正式生产更新

| 项目 | 当前状态 |
|---|---|
| 选定源码、六项CI与发行附件摘要 | 已验证 |
| `D:\MCU Studio`实际通过内置更新从0.1.0升级至0.2.0 | 公开下载、大小与SHA校验、原目录安装成功 |
| 生产升级后的自动重启 | 失败，旧冻结进程环境继承导致Python DLL路径失效 |
| Windows桌面正常启动与原工程恢复 | 已恢复，实际健康接口为0.2.0，原工程已导入 |

生产交接记录位于`artifacts/public-update-v0.2.json`。原生错误对话框记录旧临时目录`_MEI0000beb02`的Python DLL加载失败。0.2.0自动重启验收结果为失败。0.2.1为安装helper及桌面新引擎设置PyInstaller独立实例环境，已通过正式包的陈旧环境原生复验，以及本机0.2.0至0.2.1的公开更新、安装和自动重启。补验记录见`artifacts/public-update-v0.2.1.json`，对应新版验证文档随0.2.1发行。
