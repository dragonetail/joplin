---
sidebar_position: 0.5
---

# 安装

提供三种类型的应用程序：**桌面版**（Windows、macOS 和 Linux）、**移动版**（Android 和 iOS）和**终端版**（Windows、macOS、Linux 和 FreeBSD）。所有应用程序都有类似的用户界面，并且可以相互同步。

## 桌面应用程序

操作系统 | 下载
---|---
Windows (32 和 64-bit) | <a href='https://objects.joplinusercontent.com/v3.3.9/Joplin-Setup-3.3.9.exe?source=JoplinWebsite&type=New'><img alt='Get it on Windows' width="134px" src='https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/BadgeWindows.png'/></a>
macOS | <a href='https://objects.joplinusercontent.com/v3.3.9/Joplin-3.3.9.dmg?source=JoplinWebsite&type=New'><img alt='Get it on macOS' width="134px" src='https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/BadgeMacOS.png'/></a>
macOS M1 (Apple Silicon) | <a href='https://objects.joplinusercontent.com/v3.3.9/Joplin-3.3.9-arm64.DMG?source=JoplinWebsite&type=New'><img alt='Get it on macOS M1 (Silicon)' width="134px" src='https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/BadgeMacOSM1.png'/></a>
Linux | <a href='https://objects.joplinusercontent.com/v3.3.9/Joplin-3.3.9.AppImage?source=JoplinWebsite&type=New'><img alt='Get it on Linux' width="134px" src='https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/BadgeLinux.png'/></a>

**在 Windows 上**，您也可以使用<a href='https://objects.joplinusercontent.com/v3.3.9/JoplinPortable.exe?source=JoplinWebsite&type=New'>便携版</a>。[便携式应用程序](https://en.wikipedia.org/wiki/Portable_application)允许将软件安装在便携设备上，如 USB 闪存驱动器。只需将 JoplinPortable.exe 文件复制到该 USB 闪存驱动器上的任何目录中；应用程序将在可执行文件旁边创建一个名为 "JoplinProfile" 的目录。

**在 Linux 上**，推荐使用以下安装脚本，因为它也会处理桌面图标：

<pre><code style="word-break: break-all">wget -O - https://raw.githubusercontent.com/laurent22/joplin/dev/Joplin_install_and_update.sh | bash</code></pre>

安装和更新脚本支持[以下标志](https://github.com/laurent22/joplin/blob/dev/Joplin_install_and_update.sh#L50)（在编写本文时大约在第 50 行）。

## 移动应用程序

操作系统 | 下载 | 备选下载
---|---|---
Android | <a href='https://play.google.com/store/apps/details?id=net.cozic.joplin&utm_source=GitHub&utm_campaign=README&pcampaignid=MKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1'><img alt='Get it on Google Play' style="max-height: 40px;" src='https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/BadgeAndroid.png'/></a> | 或下载 [APK 文件](https://objects.joplinusercontent.com/v3.2.7/joplin-v3.2.7.apk?source=JoplinWebsite&type=New)
iOS | <a href='https://itunes.apple.com/us/app/joplin/id1315599797'><img alt='Get it on the App Store' style="max-height: 40px;" src='https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/BadgeIOS.png'/></a> | -

## 终端应用程序

操作系统 | 方法
-----------------|----------------
macOS、Linux 或 Windows（通过 [WSL](https://msdn.microsoft.com/en-us/commandline/wsl/faq?f=255&MSPPError=-2147217396)） | **重要提示：** 首先，[安装 Node 12+](https://nodejs.org/en/download/package-manager/)。<br/><br/>`NPM_CONFIG_PREFIX=~/.joplin-bin npm install -g joplin`<br/>`sudo ln -s ~/.joplin-bin/bin/joplin /usr/bin/joplin`<br><br>默认情况下，应用程序二进制文件将安装在 `~/.joplin-bin` 下。如果需要，您可以更改此目录。或者，如果您的 npm 权限按照[此处](https://docs.npmjs.com/getting-started/fixing-npm-permissions#option-2-change-npms-default-directory-to-another-directory)（选项 2）所述设置，那么只需运行 `npm -g install joplin` 即可。

要启动它，请输入 `joplin`。

有关使用信息，请参阅完整的 [Joplin 终端应用程序文档](https://joplinapp.org/help/apps/terminal/)。

## 网页剪藏器

网页剪藏器是一个浏览器扩展，允许您保存网页和浏览器中的截图。有关如何安装和使用它的更多信息，请参阅 [Web Clipper 帮助页面](https://github.com/laurent22/joplin/blob/dev/readme/apps/clipper.md)。

## 非官方替代发行版

有许多非官方的替代 Joplin 发行版。如果您不想或无法使用 AppImage 或任何其他官方支持的版本，那么您可能希望考虑这些。

但是，这些版本带有一个警告，即它们不被官方支持，因此某些问题可能不会被主项目支持。相反，支持请求、错误报告和一般建议需要发送给这些发行版的维护者。

社区维护的这些发行版列表可以在这里找到：[非官方 Joplin 发行版](https://discourse.joplinapp.org/t/unofficial-alternative-joplin-distributions/23703) 