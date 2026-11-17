# Third-party components

MCU Studio's application implementation is privately maintained by its owner. Redistributed components retain their respective licenses and copyright notices.

## Renode 1.17.0

The ARM execution backend runs as a separate Renode process. The unmodified official Windows or Linux portable runtime is accompanied by the MCU Studio peripheral assembly. Renode's MIT license is retained with the upstream runtime under `runtime/renode/licenses/renode-license`. Its dependencies retain the license files supplied in `runtime/renode/licenses/`.

Renode's tlib CPU translation library is LGPL 2.1 with file-specific notices. The runtime keeps its replaceable library files and original license documents. The exact corresponding source archive `renode_1.17.0_source.tar.xz` is provided alongside each MCU Studio release. Its SHA-256 is `65170aa79966044704ddef274568ed8b6e78b9f08edde578de33befee28a1eee`. The source archive includes upstream dependencies and build materials.

- [Renode project](https://github.com/renode/renode)
- [Official version 1.17.0](https://github.com/renode/renode/releases/tag/v1.17.0)
- [MCU Studio downloads and third-party source](https://github.com/Epiphany-Lucky/mcu-studio-releases/releases)

The published package manifest records runtime files. The build manifest `runtime/renode-runtime.json` records the exact official archives, lengths and SHA-256 values.

## Desktop and packaging components

- Microsoft .NET and WPF runtime license files are retained in `licenses/dotnet-LICENSE.txt` and `licenses/dotnet-ThirdPartyNotices.txt`.
- Microsoft Edge WebView2 SDK license and notice files are retained in `licenses/WebView2-LICENSE.txt` and `licenses/WebView2-NOTICE.txt`. The system provides the Edge WebView2 Runtime.
- Python is distributed under the Python Software Foundation license retained in `licenses/Python-LICENSE.txt`.
- PyInstaller uses its bootloader distribution exception. Its terms are retained in `licenses/PyInstaller-COPYING.txt`.
- Inno Setup 6.7.4 supplies the Windows installer engine. Its license is retained in `licenses/Inno-Setup-LICENSE.txt`.
- WiX Toolset 5.0.2 and its UI extension supply MSI authoring and the original directory-validation custom action. Their Microsoft Reciprocal License is retained in `licenses/WiX5-LICENSE.txt`. The corresponding upstream source is available at [WiX 5.0.2 source](https://github.com/wixtoolset/wix/tree/aa65968c419420d32e3e1b647aea0082f5ca5b78). MCU Studio's migration bridge and recovery helper are separate original files using the Windows Installer API and .NET 10 runtime.
- Linux desktop integration uses the operating system's default browser and `xdg-open`.

The sample firmware and board configuration are part of the educational resources shipped by the project owner. External educational material is credited in the user documentation. Third-party embedded libraries, where included, retain their own notices.
