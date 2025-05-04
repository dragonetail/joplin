# 同步您的笔记

Joplin允许您使用各种文件托管服务同步您的数据。支持的云服务如下：

## 设置Joplin Cloud同步

[Joplin Cloud](https://joplinapp.org/plans/)是专为Joplin设计的网络服务。除了同步您的数据外，它还允许您将笔记发布到互联网，或与朋友、家人或同事共享笔记本。与其他服务相比，Joplin Cloud还具有一些性能改进，允许更快的同步。

要使用它，请转到配置屏幕，然后进入同步部分。在同步目标列表中，选择"Joplin Cloud"。输入您的电子邮件和密码，您就可以使用Joplin Cloud了。

## 设置Dropbox同步

在配置屏幕中选择"Dropbox"作为同步目标。然后，要启动同步过程，请点击侧边栏中的"同步"按钮并按照说明进行操作。

## 设置Nextcloud同步

Nextcloud是一个自托管的私有云解决方案。要设置它，请转到配置屏幕并选择Nextcloud作为同步目标。然后输入WebDAV URL（要获取它，请转到您的Nextcloud页面，点击页面左下角的设置并复制URL）。请注意，这必须是**完整的URL**，例如，如果您希望笔记位于`/Joplin`下，则URL将类似于`https://example.com/remote.php/webdav/Joplin`（注意"/Joplin"部分）。并且**确保在Nextcloud中创建"/Joplin"目录**。最后设置用户名和密码。如果不起作用，请[参阅此说明](https://github.com/laurent22/joplin/issues/61#issuecomment-373282608)了解更多详情。

## 设置OneDrive或WebDAV同步

OneDrive和WebDAV也作为同步服务受到支持。请参阅[同步文档](https://joplinapp.org/help/apps/sync/)了解更多信息。

## 使用端到端加密

Joplin在所有应用程序上都支持端到端加密（E2EE）。E2EE是一种只有数据所有者才能读取数据的系统。它防止潜在的窃听者 - 包括电信提供商、互联网提供商，甚至Joplin的开发人员能够访问数据。请参阅[端到端加密教程](https://joplinapp.org/help/apps/sync/e2ee)了解有关此功能的更多信息以及如何启用它。 