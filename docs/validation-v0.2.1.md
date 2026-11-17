# MCU Studio 0.2.1 验证记录

记录日期为2026年11月17日。0.2.1修复软件升级后继承旧PyInstaller临时目录环境导致自动重启失败的问题。本记录随正式发行包和生产更新验收补齐。

## 正式构建来源

选定源码提交为 `643ddae4fd65170bf1b21906ef533c352174c8dd`，对应CI为 `44017458836`，全部六项任务成功。CI链接及原始日志受私有源码仓库访问权限控制。[完整CI记录](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836)

| CI任务 | 当前结果 | 原始记录 |
|---|---|---|
| Windows源码验证 | 成功 | [Windows任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836/job/133818725169) |
| Ubuntu 22.04源码验证 | 成功 | [Linux任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836/job/133818725063) |
| Windows安装器独立验证 | 成功 | [安装器任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836/job/133818724830) |
| Windows发行包与完整安装验证 | 成功 | [Windows打包任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836/job/133824380834) |
| Linux发行包与安装验证 | 成功 | [Linux打包任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836/job/133824380845) |
| 第三方对应源码归档 | 成功 | [第三方源码任务](https://github.com/Epiphany-Lucky/mcu-studio/actions/runs/44017458836/job/133824380929) |

## 源码测试结果

| 环境与入口 | 实际结果 |
|---|---|
| Windows源码测试 | 发现123项，通过113项，跳过10项Linux专用用例，零失败，195.166秒 |
| Linux源码测试 | 发现123项，通过122项，跳过1项Windows PowerShell执行用例，零失败，157.024秒 |
| Windows源码服务 | 16项固件执行流程、26项浏览器验收通过 |
| Linux源码服务 | 16项固件执行流程、26项浏览器验收通过 |

固件验证通过Renode后端执行已编译的ARM映像，检查真实指令执行、串口返回、GPIO变化及显示像素。两平台浏览器记录均无JavaScript错误。输入捕获实测周期为1000微秒、高电平为250微秒，断线后捕获停止，重新接线后恢复。汇总来自本次CI归档 `artifacts/ci-44017458836/source-proof/source-audit-summary.json`。

## 系统MSI与迁移

正式MSI采用 `perMachine` x64系统安装，内嵌CAB，默认安装到64位Program Files，支持自选目录。安装、修复和更新需要Windows管理员授权。安装标记使用HKLM64，记录安装技术、目录、ProductCode、版本及 `Scope=machine`。EXE保留当前用户安装与更新通道。

本次独立安装器任务采用随机测试身份 `Msifd7abc3b2177`，23项检查全部通过。实际Windows Installer上下文为4，即机器范围，用户SID为空。公共快捷方式、新装、丢失文件修复、主要升级、降级拦截、同目录EXE覆盖拦截、卸载及用户工程保留均通过。

主要升级分别在文件写入后、登记写入后及产品发布后注入失败。三次均先完成旧产品移除，失败点旧产品状态为 `-1`。回滚完成后旧产品恢复为已安装状态 `5`，新产品状态为 `-1`，旧版文件和登记恢复，新版独有文件清除，用户工程保留。产品发布后的探针位于实际 `InstallExecute` 之后。

Inno迁移在卸载旧版前完成恢复安装包长度及SHA-256核验和工程备份。准备失败时原安装保留。文件写入后、登记写入后和产品发布后三个失败点均恢复原Inno版本、原目录、登记及工程。正常迁移只保留单一有效安装登记。默认系统目录迁移和显式选择原目录的迁移均通过。

本轮预验证结果为 `artifacts/ci-44017458836/installer-preflight/msi-smoke-Msifd7abc3b2177/result.json`。Windows完整发行任务又以身份 `Msi5ac5489afd5f` 重复通过全部23项生命周期检查。完整0.2.1程序实际安装到中文空格路径后，GPIO、CH340、OLED和LCD四项ARM固件及工程导出／恢复往返通过。该轮清理错误和迁移日志收集错误列表均为空。

## 实际更新helper

本次CI的EXE与系统MSI更新helper均实际执行安装器SHA-256核验、等待应用退出、中文空格目标目录安装、安装退出码检查和应用重启见证。两种安装器返回码均为0。系统MSI记录请求管理员权限，实际使用管理员令牌，HKLM五项标记正确，最后正常卸载独立测试产品。测试版本为9.0.0，生产目录未被触及。

实际记录为 `artifacts/ci-44017458836/installer-preflight/self-update-helper-smoke.json`，整体结果及MSI子项均为成功。正式便携包继承旧冻结环境的原生启动结果见下节，公开发行附件校验和生产更新验收待补入。

## 发行包和原生验收

| 项目 | 当前状态 |
|---|---|
| Windows冻结发行包16项固件及26项浏览器验收 | 全部通过，JavaScript错误为0 |
| Windows实际安装EXE／MSI后的四项ARM固件和工程往返 | 两种安装通道均全部通过，EXE重装及卸载保留用户工程通过 |
| Linux实际安装DEB后的固件、浏览器及启动器验收 | 四项ARM固件与工程往返、16项完整固件流程、26项浏览器验收、启动器复用与正常退出均通过，apt卸载通过 |
| 正式Windows便携包在旧PyInstaller环境下的原生启动 | 通过，WPF与后端正常启动，关闭后退出码0、后端停止 |
| 正式附件字节数与本地SHA-256 | 已核对本次CI下载的四项Windows附件和两项Linux附件 |
| 公开发行附件digest核对 | 待完成 |

Linux验收使用实际安装到 `/opt/mcu-studio` 的0.2.1程序。浏览器无JavaScript错误，启动器复用进程后实际执行460316条ARM指令，并正常退出。DEB与便携归档各包含2114个文件和2113项清单记录，逐文件SHA-256全部一致，两种容器中的软件目录树相同。发行内容审计记录179个默认端点、144个物理引脚、114个GPIO、55份固件映像及28项实验菜单，私有源码泄漏检查为0。对应记录为 `linux-install-proof/linux-install-audit-summary.json` 及 `linux-payload-audit.json`，均归档在本次CI目录下。

本机原生验收使用本次CI正式Windows便携包，携带旧更新进程遗留的PyInstaller环境启动。实际WPF窗口和后端正常运行，LCD写入76800次并呈现4种颜色，页面错误为0。120 DPI下窗口尺寸为1600×980，完整处于1920×1020工作区。窗口关闭后退出码为0，后端停止。原始记录为 `artifacts/native-ci-v0.2.1.json`，完整窗口截图为 `native-ci-v0.2.1.png`，内容截图为 `native-ci-v0.2.1-content.png`。

## 发行附件摘要

以下摘要来自本次CI正式产物，并与 `artifacts/ci-44017458836/release-inventory.json` 逐项核对。Windows四项附件所在GitHub artifact为 `14300413835`，外层归档实际长度为602705818字节，SHA-256为 `a16cfe3fd6c73f33014478a82310f80db957da217c0a2020684766944253c1e4`，与GitHub返回的artifact digest一致。`SHA256SUMS-0.2.1.txt`已随附件上传发行草稿，公开发行附件上传后的digest核对待完成。

| 文件 | 字节数 | SHA-256 |
|---|---:|---|
| `MCU-Studio-0.2.1-Setup-x64.msi` | 218134675 | `a101ebfd16c7fbc9f557cad399fb6083674e77ae589b992977efb1433ed23aad` |
| `MCU-Studio-0.2.1-Setup-x64.exe` | 127723624 | `f8cf3e6f1ff74370caf953e99da20f59221cb8eb166ff7b1d61b0df8c6d2596e` |
| `MCU-Studio-0.2.1-win-x64.zip` | 181653538 | `1dd24de7854bcc9a6382d5d854a61d6b5929f0b4adb5bacf73d561a3667ac39b` |
| `MCU-Studio-0.2.1-html-local.zip` | 120305865 | `0b7a3942134b89faf4074c5965d3f188f32d2dc36b3c4b81c4717678f0406721` |
| `mcu-studio_0.2.1_amd64.deb` | 66959696 | `26f02ec24563be305e73f76e5cf0371112e5215c399f0b62dcdcd2902105af5d` |
| `MCU-Studio-0.2.1-linux-x64.tar.gz` | 87568939 | `02dd445c23fcf3fedf20d24339dc99d5d870aef542971fd90b43137d672a6ee6` |
| `renode_1.17.0_source.tar.xz` | 329418672 | `65170aa79966044704ddef274568ed8b6e78b9f08edde578de33befee28a1eee` |

## 证据归档

以下相对路径以验收工作区为根。

- 源码测试汇总为 `artifacts/ci-44017458836/source-proof/source-audit-summary.json`。
- Windows源码固件与浏览器结果分别为 `artifacts/ci-44017458836/source-proof/Windows/source-firmware-Windows.json` 和 `source-browser-Windows/result.json`。
- Linux源码固件与浏览器结果分别为 `artifacts/ci-44017458836/source-proof/Linux/source-firmware-Linux.json` 和 `source-browser-Linux/result.json`。
- 独立系统MSI预验证为 `artifacts/ci-44017458836/installer-preflight/msi-smoke-Msifd7abc3b2177/result.json`。
- 实际更新helper为 `artifacts/ci-44017458836/installer-preflight/self-update-helper-smoke.json`。
- Windows发行包安装验收汇总为 `artifacts/ci-44017458836/windows-install-verification/windows-install-audit-summary.json`。
- Windows冻结固件与浏览器结果为 `artifacts/ci-44017458836/windows-install-verification/frozen-windows-firmware.json` 和 `frozen-windows-browser/result.json`。
- 完整MSI生命周期及安装后固件结果为 `artifacts/ci-44017458836/windows-install-verification/msi-smoke-Msi5ac5489afd5f/result.json`，EXE结果为同归档内 `installer-smoke-2887c1d87ca8/result.json`。
- Linux已安装程序验收汇总为 `artifacts/ci-44017458836/linux-install-proof/linux-install-audit-summary.json`，两种Linux容器内容核对为 `artifacts/ci-44017458836/linux-payload-audit.json`。
- Windows附件摘要为 `artifacts/ci-44017458836/windows-release-inventory.json`，传输归档校验为 `windows-artifact-transport-audit.json`。
- 正式Windows便携包的旧冻结环境原生验收为 `artifacts/native-ci-v0.2.1.json`、`native-ci-v0.2.1.png` 及 `native-ci-v0.2.1-content.png`。

## 正式生产更新

| 项目 | 当前状态 |
|---|---|
| 0.2.0自动重启缺陷 | 已复现并定位到旧冻结环境继承 |
| 0.2.1源码修复及两平台源码验证 | 已通过 |
| `D:\MCU Studio`通过软件内更新升级至0.2.1 | 待完成 |
| 升级后自动启动、后端健康和原工程恢复 | 待完成 |

生产更新将使用正式公开发行附件完成，结果待验收后补入。
