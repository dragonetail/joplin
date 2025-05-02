# OneDrive 同步

当与 OneDrive 同步时，Joplin 会在 OneDrive 中创建一个子目录，位于 /Apps/Joplin，并在其中读取/写入笔记和笔记本。应用程序无法访问此目录之外的任何内容。

在**桌面应用程序**或**移动应用程序**中，在[配置界面](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)中选择 "OneDrive" 作为同步目标。然后，要启动同步过程，请点击侧边栏中的"同步"按钮并按照说明进行操作。

在**终端应用程序**中，要启动同步过程，请输入 `:sync`。系统会要求您点击一个链接来授权应用程序（只需输入您的 Microsoft 凭据 - 您不需要注册 OneDrive）。 