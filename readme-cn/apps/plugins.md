# 插件

**桌面端**和**移动端**应用程序可以通过插件扩展其标准功能。这些插件遵循Joplin的[插件API](https://joplinapp.org/api/references/plugin_api/classes/joplin.html)，可以在应用程序中通过[配置屏幕](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)的`插件`页面进行安装和配置。在此菜单中，您可以搜索上传到插件仓库的插件，也可以使用"Joplin插件归档"(*.jpl)文件手动安装插件。

应用程序重新加载后，插件将出现在插件菜单中，您可以在此处打开/关闭或完全移除它们。

## 插件仓库

插件托管在[Joplin插件](https://github.com/joplin/plugins)仓库中。应用程序从该位置搜索和安装插件，但也支持手动下载和安装.jpl文件。

请查看[Joplin论坛的"插件"分类](https://discourse.joplinapp.org/c/plugins/18)，了解插件说明文档、测试版/开发中的插件以及社区讨论。

## 安装插件

要安装插件，只需在[配置屏幕](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)的`插件`页面内的搜索框中搜索插件名称。输入<kbd>空格</kbd>将显示所有插件；在桌面端，您还可以通过点击`插件工具`"齿轮"按钮并选择`浏览所有插件`来浏览仓库。  
[推荐插件](https://github.com/joplin/plugins/blob/master/readme/recommended.md#recommended-plugins)用金色皇冠图标标记，这些插件已经过Joplin团队的审核和推荐。  
要安装插件，只需按下其`安装`按钮，然后应用程序将需要重新启动以完成安装，并会提示您这样做。  

或者，要手动安装插件，首先将插件下载为`.jpl`文件。接下来，
- **在桌面端**：按下`插件工具`"齿轮"按钮并选择`从文件安装`，然后选择下载的`.jpl`文件。或者，您可以将`.jpl`复制到您的配置文件的`plugins`目录`~/.config/joplin-desktop/plugins`（此路径在您的设备上可能不同 - 请在[配置屏幕](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)的`选项`页面顶部检查）。重新启动应用程序时，插件将自动加载和执行。您可能需要检查Joplin是否只是最小化到系统托盘/通知区域，而不是完全关闭。
- **在Android上**，在配置屏幕的"插件"选项卡的"高级设置"下，有一个`从文件安装`按钮。

:::note

为遵守AppStore指南，iOS应用只允许安装推荐的插件。

:::

## 管理插件

在Joplin插件页面中，您可以使用切换控件打开或关闭各个插件。更改插件状态后，必须重启Joplin，您可能需要检查Joplin是否只是最小化到系统托盘/通知区域，而不是完全关闭。

由于插件集成到应用程序本身，每个插件可能在Joplin中有自己的配置选项，并可能以多种不同方式执行。确保您完整阅读作者的文档，以了解每个插件的设计配置和使用方式。

## 更新插件

可以从Joplin应用程序内自动更新插件。当有更新可用时，设置菜单的`插件`页面上会显示一个橙色的`更新`按钮。按下此按钮进行更新，完成后它会提示您重新启动应用程序。

## 卸载插件

在Joplin插件页面中，您可以点击插件上的"删除"按钮，将其从列表中移除。Joplin必须重新启动才能完成此操作。您可能需要检查Joplin是否只是最小化到系统托盘/通知区域，而不是完全关闭。

或者，您也可以简单地从插件目录中删除*.jpl文件（请参阅安装插件部分）。此更改将在应用程序重新启动后生效。

## 开发

有关插件API的文档以及有关插件开发的文档。请查看[Joplin API概述](https://github.com/laurent22/joplin/blob/dev/readme/api/index.md)页面了解这些项目。
关于插件开发的社区讨论和帮助，请参阅[Joplin论坛的"插件开发"分类](https://discourse.joplinapp.org/c/development/plugins/19)。
其他资源可以在[Joplin帮助页面](https://joplinapp.org/help/)的`Joplin API - 入门`和`Joplin API - 参考`分类中找到。 