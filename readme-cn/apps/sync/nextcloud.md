# Nextcloud 同步

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/nextcloud-logo-background.png" width="100" align="left"> <a href="https://nextcloud.com/">Nextcloud</a> 是一个自托管的私有云解决方案。它可以存储文档、图片和视频，还可以存储日历、密码和其他无数内容，并可以将它们同步到您的笔记本电脑或手机。由于您可以托管自己的 Nextcloud 服务器，您同时拥有设备上的数据和用于同步的基础设施。因此，它非常适合 Joplin。该平台也得到了很好的支持，拥有强大的社区，所以它很可能会存在一段时间 - 由于它是开源的，无论如何它不是一个可以被关闭的服务，只要有人愿意，它可以在服务器上存在。

在**桌面应用程序**或**移动应用程序**中，进入[配置界面](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)并选择 Nextcloud 作为同步目标。然后输入 WebDAV URL（要获取它，在 Nextcloud 中的文件视图页面左下角点击设置），这通常是 `https://example.com/nextcloud/remote.php/webdav/Joplin` 或 `https://example.com/nextcloud/remote.php/dav/files/<nextcloud-username>/Joplin` (**确保在 Nextcloud 中创建 "Joplin" 目录**)，并设置用户名和密码。如果它不起作用，请[参阅此说明](https://github.com/laurent22/joplin/issues/61#issuecomment-373282608)获取更多详细信息。    

在**终端应用程序**中，您需要设置 `sync.target` 配置变量以及所有 `sync.5.path`、`sync.5.username` 和 `sync.5.password` 配置变量，分别对应 Nextcloud WebDAV URL、您的用户名和密码。这可以通过命令行模式完成：

	:config sync.5.path https://example.com/nextcloud/remote.php/webdav/Joplin
	:config sync.5.username YOUR_USERNAME
	:config sync.5.password YOUR_PASSWORD
	:config sync.target 5

如果同步不起作用，请查看应用程序配置目录中的日志 - 这通常是由于 URL 或密码配置错误。日志应该指出确切的问题所在。 