# 常见问题解答

## Windows 上安装程序卡住

如果应用程序未正确卸载，安装程序可能会卡住。要解决这个问题，您需要清理注册表中的剩余条目。请按照以下步骤操作：

- 按下 Win + R（Windows 键 + R）
- 输入 "regedit.exe"
- 导航到 `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Uninstall`
- 在那里，您将看到一个或多个文件夹。逐个打开它们，找到 Joplin 的那个。其中一个条目应该是 "DisplayName"，值为 "Joplin x.x.x"。
- 找到后，删除该文件夹。

现在尝试重新安装，应该可以正常工作了。

更多信息请参见：https://github.com/electron-userland/electron-builder/issues/4057

## 如何向 Linux 安装脚本传递参数？

您可以使用以下命令向安装脚本传递[参数](https://github.com/laurent22/joplin/blob/dev/Joplin_install_and_update.sh#L37)。

<pre><code style="word-break: break-all">wget -O - https://raw.githubusercontent.com/laurent22/joplin/dev/Joplin_install_and_update.sh | bash -s -- --argument1 --argument2</code></pre>

## Linux 上桌面应用程序无法启动

如果您直接下载了 AppImage 而没有通过推荐的脚本安装，则可能当前不允许执行，需要手动设置这些权限（请参阅 [AppImage 用户指南](https://docs.appimage.org/introduction/quickstart.html#how-to-run-an-AppImage)）。

如果执行权限正确但仍然无法启动，那么您的系统可能没有 AppImage 运行所需的 `libfuse2` 库。这个库要求是 AppImage 格式固有的，而不是 Joplin 特有的。更多信息请参阅[这个论坛帖子](https://discourse.joplinapp.org/t/appimage-incompatibility-in-ubuntu-22-04/25173)，其中包含有关此问题的更多详细信息和一个 [Ubuntu 特定的修复方法](https://discourse.joplinapp.org/t/appimage-incompatibility-in-ubuntu-22-04/25173/12)。

## 如何在外部文本编辑器中编辑我的笔记？

编辑器命令（可能包括参数）定义了将使用哪个编辑器打开笔记。如果没有提供，它将尝试自动检测默认编辑器。如果这没有效果或您想为 Joplin 更改它，您需要在"首选项 -> 文本编辑器命令"中进行配置。

一些配置示例如下：（#后面是注释）

Linux/Mac:

```bash
subl -n -w      # 在新窗口(-n)中打开 Sublime (subl) 并等待关闭(-w)
code -n --wait  # 在新窗口(-n)中打开 Visual Studio Code (code) 并等待关闭(--wait)
gedit --new-window    # 在新窗口中打开 gedit（Gnome 文本编辑器）
xterm -e vim    # 打开一个新终端并打开 vim。可以替换为其他
                # 替代终端(gnome-terminal, terminator 等)
                # 或终端文本编辑器(emacs, nano 等)
open -a <application> # 仅限 Mac：打开一个 GUI 应用程序
```

Windows:

```bash
subl.exe -n -w      # 在新窗口(-n)中打开 Sublime (subl) 并等待关闭(-w)
code.exe -n --wait  # 在新窗口(-n)中打开 Visual Studio Code 并等待关闭(--wait)
notepad.exe         # 在新窗口中打开记事本
notepad++.exe --openSession   # 在新窗口中打开 Notepad++
```

请注意，包含编辑器可执行文件的目录路径必须存在于您的 PATH 变量中（[Windows](https://www.computerhope.com/issues/ch000549.htm), [Linux/Mac](https://opensource.com/article/17/6/set-path-linux)）。如果没有，则必须提供可执行文件的完整路径。

## 当我在 vim 中打开笔记时，光标不可见

这似乎是由于 .vimrc 中的设置 `set term=ansi` 导致的。删除它应该可以解决问题。更多信息请参见 https://github.com/laurent22/joplin/issues/147。

## 更改 WebDAV URL 后，我的所有笔记都被删除了！

更改 WebDAV URL 时，请确保新位置与旧位置具有完全相同的内容（即将所有 Joplin 数据复制到新位置）。否则，如果新位置上没有任何内容，Joplin 会认为您已删除所有数据，并将继续在本地也删除它。因此，要更改 WebDAV URL，请按照以下步骤操作：

1. 以防万一出错，请备份您的 Joplin 数据。例如，导出为 JEX 存档。
2. 从 Joplin 客户端（例如，从桌面客户端）最后同步一次您的所有数据。
3. 关闭 Joplin 客户端。
4. 在您的 WebDAV 服务上，将所有 Joplin 文件从旧位置复制到新位置。确保同时复制 `.resource` 目录，因为它包含您的图片和其他附件。
5. 完成后，再次打开 Joplin 并更改 WebDAV URL。
6. 同步以验证一切正常工作。
7. 对所有需要同步的其他 Joplin 客户端执行步骤 5 和 6。

## 我不小心删除了一些笔记，并且没有备份

如果您知道 `NOTE_ID` 并启用了笔记历史记录，您可以从命令面板运行命令 `restoreNoteRevision`，例如 `restoreNoteRevision 66457326a6ba4adeb4be8ce05e37af0d`。Joplin 随后将确认恢复是否成功，并将笔记放入"已恢复的笔记"笔记本中。
如果您不知道 `NOTE_ID`，则可以在 Joplin sqlite 数据库中的 `deleted_items` 或 `revisions` 表中找到它作为 `item_id`。这将需要手动检查 `title_diff` 和 `body_diff` 字段，以检查您正在定位的 `ITEM/NOTE_ID` 是否正确。
您应该先复制数据库，以避免在实时数据库中进行任何意外更改。
更多信息请前往[这里](https://discourse.joplinapp.org/t/restoring-deleted-notes/21304)。

## 如何在 Android 上轻松输入 Markdown 标签？

您可以使用特殊键盘，如 [Multiling O Keyboard](https://play.google.com/store/apps/details?id=kl.ime.oh&hl=en)，它有创建 Markdown 标签的快捷方式。[此帖子中有更多信息](https://discourse.joplinapp.org/t/android-create-new-list-item-with-enter/585/2?u=laurent)。

## 初始同步非常慢，如何加速？

每当导入大量笔记时，例如从 Evernote 导入，第一次同步可能需要很长时间才能完成。有各种技术可以加速这个过程（如果您不想简单地等待同步完成），这些技术在[这篇文章](https://discourse.joplinapp.org/t/workaround-for-slow-initial-bulk-sync-after-evernote-import/746?u=laurent)中概述。

## 移动应用上不显示所有笔记、文件夹或标签

Joplin 在移动设备上没有后台同步功能。当 Joplin 关闭、发送到后台或设备进入睡眠状态（显示关闭）时，同步会中断。

## 如何检查同步状态？

前往同步页面。您可以在桌面应用程序的 `帮助 > 同步状态` 下找到它，在移动应用上则在 `配置 > 工具 > 同步状态` 下找到。

`total items` = 总共有多少项目需要同步。  
`synced items` = 已经上传或下载了多少个项目。

如果 `total items` 和 `synced items` 相等，则所有数据已同步。同时，所有设备应该有相同的 `total items`。

## 是否可以在同步目标中使用真实的文件和文件夹名称？

不幸的是，这是不可能的。Joplin 使用开放格式与文件系统同步，但这并不意味着同步文件是供用户编辑的。该格式设计为高效和可靠，而不是用户友好的（它不可能两者兼具），这是无法改变的。Joplin 同步目录基本上只是一个数据库。

## 是否可以设置密码来限制对 Joplin 的访问？

在移动设备上，您可以启用生物识别锁来保护对 Joplin 应用程序的访问。在桌面端，我们目前不支持这一功能。但是，有一个关于它的未解决问题：https://github.com/laurent22/joplin/issues/289

## 为什么我的 WebDAV 主机不工作？

### Strato 中的"Forbidden"错误

例如：

    MKCOL .sync/: Unknown error 2 (403): <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
    <html><head>
    <title>403 Forbidden</title>
    </head><body>
    <h1>Forbidden</h1>
    <p>You don't have permission to access /.sync/
    on this server.</p>
    </body></html>

在这种情况下，[确保您输入了正确的 WebDAV URL](https://github.com/laurent22/joplin/issues/309)。

### 不支持以下 WebDAV 主机

- Jianguoyun（见 [Github issue](https://github.com/laurent22/joplin/issues/4294)）
- pCloud（见 [Forum thread](https://discourse.joplinapp.org/t/feature-request-pcloud-synchronisation/3530/51)）

### Nextcloud 同步不工作

- 检查您的用户名和密码。**手动输入**（不要复制粘贴）并再次尝试。
- 检查 WebDAV URL - 要获取正确的 URL，请转到 Nextcloud，在左侧边栏中，点击"设置"并从那里复制 WebDAV URL。**不要忘记将您创建的文件夹添加到该 URL**。例如，如果 WebDAV URL 的基础是 "https://example.com/nextcloud/remote.php/webdav/" 并且您希望笔记在 "Joplin" 目录中同步，则需要提供 URL "https://example.com/nextcloud/remote.php/webdav/Joplin" **并且您需要自己创建 "Joplin" 目录**。
- 您是否在 Nextcloud 上启用了 **2FA**（多因素认证）？在这种情况下，您需要在 Nextcloud 管理界面中为 Joplin [创建一个应用密码](https://github.com/laurent22/joplin/issues/1453#issuecomment-486640902)。

## 为什么我的同步和加密密码在更新 Joplin 后消失了？

- 从版本 2.12 开始，Joplin 原生支持 M1 Mac！因此，在这些系统上升级 Joplin 会导致 Joplin 无法访问旧版本应用程序存储在系统钥匙串中的信息。这包括同步和加密密码。
- 重新输入密码应该可以解决相关的同步和加密问题。

## 如何在 Android 上使用自签名 SSL 证书？

如果您想使用 https 服务但不能或不想使用受信任的证书颁发机构（如"Let's Encrypt"）签名的 SSL 证书，可以生成自定义 CA 并使用它签署您的证书。您可以使用 [openssl](https://gist.github.com/fntlnz/cf14feb5a46b2eda428e000157447309) 生成 CA 和证书，但我喜欢使用 [mkcert](https://github.com/FiloSottile/mkcert) 这个工具，因为它简单易用。最后，您必须将您的 CA 证书添加到 Android 设置中，这样 Android 才能将您使用 CA 签名的证书识别为有效的（[链接](https://support.google.com/nexus/answer/2844832?hl=en-GB)）。

## 如何在 Windows 上重启 Joplin（以便某些更改生效）？

如果启用了 `显示托盘图标`，关闭 Joplin 窗口不会退出应用程序。要正确重启应用程序，必须执行以下操作之一来退出 Joplin：

- 点击菜单中的 `文件`，然后点击 `退出`
- 右键点击 Joplin 托盘图标，然后点击 `退出`

此外，可以使用 Windows 任务管理器来验证 Joplin 是否仍在运行。

## 在 iOS 备份到 Mac 时笔记本和笔记是否被备份？

在[备份到 Mac](https://support.apple.com/guide/mac-help/back-up-and-restore-your-device-mchla3c8ed03/mac) 时，iOS 上的笔记本和笔记不会被备份。

## 为什么命名为 Joplin？

该应用程序以作曲家和钢琴家 [Scott Joplin](https://en.wikipedia.org/wiki/Scott_Joplin) 的名字命名，我经常听他的音乐。他的名字也易于记忆和输入，使其成为一个合适的选择。 