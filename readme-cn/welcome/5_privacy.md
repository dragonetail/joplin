# Joplin隐私政策

Joplin应用程序，包括Android、iOS、Windows、macOS和Linux应用程序，未经您授权不会向任何服务发送任何数据。Joplin保存的任何数据，如笔记或图像，都保存在您自己的设备上，您可以随时自由删除这些数据。

如果您选择与第三方同步，例如OneDrive或Dropbox，笔记将被发送到该账户，在这种情况下，第三方隐私政策适用。

为了提供某些功能，Joplin可能需要连接到第三方服务。您可以在应用程序设置中禁用大多数这些功能：

| 功能  | 描述   | 默认  | 可以禁用 |
| -------- | ------------- | -------- | --- |
| 自动更新 | Joplin定期连接到GitHub以检查新版本。 | 启用 | 是 |
| 地理位置 | 当您创建笔记时，Joplin在笔记属性中保存地理位置信息。 | 启用 | 是 |
| 同步 | Joplin支持跨多个设备同步您的笔记。如果您选择与第三方同步，例如OneDrive，笔记将被发送到您的OneDrive账户，在这种情况下，第三方隐私政策适用。 | 禁用 | 是 |
| Wifi连接检查 | 在移动设备上，Joplin检查Wifi连接性，以便提供仅在启用Wifi时同步数据的选项。 | 启用 | 否 <sup>(1)</sup> |
| 拼写检查字典 | 在Linux和Windows上，桌面应用程序从`redirector.gvt1.com`下载拼写检查字典。 | 启用 | 是 <sup>(2)</sup> |
| 插件库 | 桌面应用程序从[官方GitHub仓库](https://github.com/joplin/plugins)下载可用插件列表。如果此仓库无法访问（例如在中国），应用程序将尝试从[各种镜像](https://github.com/laurent22/joplin/blob/8ac6017c02017b6efd59f5fcab7e0b07f8d44164/packages/lib/services/plugins/RepositoryApi.ts#L22)获取插件列表，在这种情况下，插件屏幕[工作方式略有不同](https://github.com/laurent22/joplin/issues/5161#issuecomment-925226975)。 | 启用 | 否
| 语音输入 | 如果您在Android上使用语音输入功能，应用程序将从https://github.com/joplin/voice-typing-models/ 或https://alphacephei.com/vosk/models 下载语言文件。 | 禁用 | 是

<sup>(1) https://github.com/laurent22/joplin/issues/5705</sup><br/>
<sup>(2) 如果禁用拼写检查器，[它将不会下载字典](https://discourse.joplinapp.org/t/new-version-of-joplin-contacting-google-servers-on-startup/23000/40?u=laurent)。</sup>

如有关于Joplin隐私政策的任何问题，请在[论坛](https://discourse.joplinapp.org/)上留言。 