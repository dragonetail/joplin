# Joplin 网页剪藏器

网页剪藏器是一个浏览器扩展，允许您从浏览器保存网页和截图。要开始使用它，请打开 Joplin 桌面应用程序，进入[配置界面](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)，打开 **网页剪藏器** 部分并按照说明进行操作。

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/WebExtensionScreenshot.png" style="max-width: 50%; border: 1px solid gray;">

## 网页剪藏器服务故障排除

网页剪藏器扩展与 Joplin 应用程序通过 Joplin 桌面应用启动的服务进行通信。

然而，某些因素可能会干扰这项服务，导致无法访问或无法启动。如果出现问题，请检查以下几点：

- 检查服务是否已启动。您可以在桌面应用的网页剪藏器选项中检查这一点。
- 检查服务使用的端口是否被防火墙阻止。您可以在桌面 Joplin 应用程序的网页剪藏器选项中找到端口号。
- 检查机器上是否有代理在运行，或确保来自网页剪藏器服务的请求已被过滤并允许。例如 https://github.com/laurent22/joplin/issues/561#issuecomment-392220191

如果以上方法都不起作用，请在[论坛](https://discourse.joplinapp.org/)或 [GitHub 问题跟踪器](https://github.com/laurent22/joplin/issues)上报告问题。

## 调试扩展

### 在 Chrome 中

为了在报告问题时提供尽可能多的信息，您可以提供来自各种 Chrome 控制台的日志。

首先，在 [chrome://extensions/](chrome://extensions/) 中启用开发者模式

- 调试弹出窗口：右键点击 Joplin 扩展图标，选择"检查弹出窗口"。
- 调试后台脚本：在 `chrome://extensions/` 中，点击"检查后台脚本"。
- 调试内容脚本：按 Ctrl+Shift+I 打开当前页面的控制台。

### 在 Firefox 中

- 在 Firefox 中打开 [about:debugging](about:debugging)。
- 确保已勾选"启用附加组件调试"复选框。
- 向下滚动到 Joplin 网页剪藏器扩展。
- 点击"调试" - 这应该会打开一个新的控制台窗口。

同时按 F12 打开常规 Firefox 控制台（Joplin 扩展的一些消息也可能出现在这里）。

现在正常使用扩展并复现您遇到的问题。

复制并粘贴调试窗口和 Firefox 控制台的内容，并将其发布到[论坛](https://discourse.joplinapp.org/)。

## 使用网页剪藏器服务

网页剪藏器服务可用于从任何其他应用程序创建、修改或删除笔记、笔记本、标签等。它公开了一个 API，提供了多个方法来管理 Joplin 的数据。有关此 API 及其使用方法的更多信息，请查看 [Joplin API 文档](https://github.com/laurent22/joplin/blob/dev/readme/api/references/rest_api.md)。 