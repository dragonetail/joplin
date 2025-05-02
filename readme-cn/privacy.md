# Joplin 隐私政策

Joplin 应用程序，包括 Android、iOS、Windows、macOS 和 Linux 应用程序，未经您的授权不会向任何服务发送任何数据。Joplin 保存的任何数据，如笔记或图片，都保存在您自己的设备上，您可以随时自由删除这些数据。

为了提供某些功能，Joplin 可能需要连接到第三方服务。您可以在应用程序设置中禁用大多数这些功能：

| 功能 | 描述 | 默认 | 可禁用 |
| -------- | ------------- | -------- | --- |
| 自动更新 | Joplin 定期连接到 `objects.joplinusercontent.com` 检查新版本。 | 启用 | 是 |
| 地理位置 | 当您创建笔记时，Joplin 会在笔记属性中保存地理位置信息。为此，它会连接到 `ipwho.is` 或 `geoplugin.net` | 启用 | 是 |
| 同步 | Joplin 支持在多个设备之间同步您的笔记。如果您选择与第三方同步，例如 OneDrive，笔记将被发送到您的 OneDrive 账户，在这种情况下适用第三方隐私政策。 | 禁用 | 是 |
| WiFi 连接检查 | 在移动设备上，Joplin 检查 WiFi 连接状态以提供仅在 WiFi 启用时同步数据的选项。 | 启用 | 否 <sup>(1)</sup> |
| 拼写检查字典 | 在 Linux 和 Windows 上，桌面应用程序从 `redirector.gvt1.com` 下载拼写检查字典。 | 启用 | 是 <sup>(2)</sup> |
| 插件仓库 | 桌面应用程序从[官方 GitHub 仓库](https://github.com/joplin/plugins)下载可用插件列表。如果无法访问此仓库（例如在中国），应用程序将尝试从[各种镜像](https://github.com/laurent22/joplin/blob/8ac6017c02017b6efd59f5fcab7e0b07f8d44164/packages/lib/services/plugins/RepositoryApi.ts#L22)获取插件列表，在这种情况下插件屏幕[工作方式略有不同](https://github.com/laurent22/joplin/issues/5161#issuecomment-925226975)。 | 启用 | 否 |
| 语音输入 | 如果您在 Android 上使用语音输入功能，应用程序将从 https://github.com/joplin/voice-typing-models/ 或 https://alphacephei.com/vosk/models 下载语言文件。 | 禁用 | 是 |
| OCR | 如果您在桌面版启用了光学字符识别，应用程序将从 https://cdn.jsdelivr.net/npm/@tesseract.js-data/ 下载语言文件。 | 禁用 | 是 |
| 崩溃报告 | 如果您启用了崩溃自动上传，应用程序会在崩溃发生时将报告上传到 Sentry。当 Sentry 初始化时，它还会连接到 `sentry.io`。 | 禁用 | 是 |

<sup>(1) https://github.com/laurent22/joplin/issues/5705</sup><br/>
<sup>(2) 如果拼写检查器被禁用，[它将不会下载字典](https://discourse.joplinapp.org/t/new-version-of-joplin-contacting-google-servers-on-startup/23000/40?u=laurent)。</sup>

如有关于 Joplin 隐私政策的任何问题，请在[论坛](https://discourse.joplinapp.org/)上留言。 