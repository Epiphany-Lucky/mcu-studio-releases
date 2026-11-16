# MCU Studio

STM32F407 嵌入式固件仿真工作台。使用 Apple 风格界面，在本地载入真实 HEX/BIN，连接虚拟外设并观察实际程序运行。

当前版本为 **0.1.0 开发预览**，目标芯片为 STM32F407ZGT6，提供 LQFP144 全引脚工作台。

## 下载

从 [官方发行页](https://github.com/Epiphany-Lucky/mcu-studio-releases/releases) 获取软件，按操作系统选择安装包或便携版。下载后可使用发行页提供的 SHA-256 清单核对文件。

| 文件 | 用途 |
|---|---|
| `MCU-Studio-0.1.0-Setup-x64.exe` | Windows 当前用户安装 |
| `MCU-Studio-0.1.0-win-x64.zip` | Windows 便携目录 |
| `MCU-Studio-0.1.0-html-local.zip` | Windows 本地浏览器工作台 |
| `mcu-studio_0.1.0_amd64.deb` | Ubuntu/Debian amd64 安装 |
| `MCU-Studio-0.1.0-linux-x64.tar.gz` | Linux 便携目录 |

Windows 桌面壳使用 Edge WebView2。Linux 版使用系统图形浏览器。运行包提供本地执行后端。[安装说明](docs/install.md)

## 使用

1. 选择配套实验，载入固件与可编辑参考跳线。
2. 点击运行，操作按键、模拟输入或串口。
3. 在接线工作台增删跳线，按固件配置选择接口路径。
4. 按需加入 OLED 或 LCD，保存工程并再次载入。
5. 使用固件入口载入自己的 Keil HEX/BIN，或共同装入多个镜像。

CH340 与 RS485 支持选择使用。RS485 方向来自固件控制的 GPIO。LCD 使用 ILI9341 SPI 240×320 RGB565，OLED 使用 SSD1306 128×64。实际接线、GPIO 模式和复用配置共同决定通信与显示。[快速开始](docs/quick-start.md)

主图和引脚表共同引出全部 144 个物理脚，含 114 个 GPIO 及独立编号的电源与专用脚。可按物理编号、端口或复用功能搜索并接线，封装顶视图支持放大至 400%。数据来自 DS8626 Rev 12 的图 14 和表 7。[ST 数据手册](https://www.st.com/resource/en/datasheet/stm32f407zg.pdf)

配套菜单提供 28 项实验，包括 GPIO、按键与 EXTI、SysTick、PWM、CH340、RS485、ADC、DAC 引脚回接、OLED、SPI Flash、LCD、Bootloader、PLL、TIM1–TIM14 和输入捕获。界面显示实际系统与总线时钟、定时器计数和通道状态。TIM1/TIM8 可观察互补输出、死区、主输出使能和刹车，边沿记录来自后端实际输出事件。

时钟路径覆盖 HSI、HSE 和 PLL，以及 AHB/APB 分频和 SysTick 的 HCLK/HCLK÷8 输入。定时器当前计数路径覆盖向上计数与边沿对齐，向下和中心对齐配置通过诊断提示。实际模型与验证结果见同版发行公告。

## 更新

软件设置内提供检查更新、下载进度、摘要校验和系统安装交接。发布时同步安装文件、公告和验证记录。源码由项目所有者在私有仓库管理，公开仓库提供软件分发与使用资料。

## 第三方组件

ARM 固件通过独立 Renode 1.17.0 后端运行。第三方许可、精确版本和对应源码资料随发行提供。自有软件与第三方组件各自遵循相应条款。[第三方声明](THIRD_PARTY_NOTICES.md)
