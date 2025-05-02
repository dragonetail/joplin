# 扩展 Joplin

Joplin提供了许多扩展点，允许第三方应用程序访问其数据或开发插件。

主要的两个扩展点是：

## 数据API

[数据API](https://github.com/laurent22/joplin/blob/dev/readme/api/references/rest_api.md)为外部应用程序提供对Joplin数据的访问。通过标准HTTP调用，可以创建、修改或删除笔记、笔记本、标签等，以及将文件附加到笔记并检索这些文件。

例如，这就是Web剪藏器与Joplin通信的方式，如果您有需要访问Joplin数据的外部应用程序，这很可能是您需要的。

要开始使用数据API，请[查看文档](https://github.com/laurent22/joplin/blob/dev/readme/api/references/rest_api.md)。

## 插件API

通过插件，您可以通过添加新功能直接修改Joplin。使用此API，您可以：

- 通过数据API访问笔记、文件夹等
- 添加视图以使用HTML/CSS/JS显示自定义数据
- 创建对话框以显示信息并从用户获取输入
- 创建新命令并将工具栏按钮或菜单项与其关联
- 获取对当前正在编辑的笔记的访问权限并修改它
- 监听各种事件并在它们发生时运行代码
- 挂钩应用程序以设置其他选项并定制Joplin的行为
- 创建模块以导出或导入数据到Joplin
- 定义新的设置和设置部分，并从插件中获取/设置它们
- 创建新的Markdown插件以渲染自定义标记
- 创建编辑器插件，在低级别修改Markdown编辑器（CodeMirror）的行为

要开始使用插件API，请查看[入门](https://github.com/laurent22/joplin/blob/dev/readme/api/get_started/plugins.md)页面或查看[TOC教程](https://github.com/laurent22/joplin/blob/dev/readme/api/tutorials/toc_plugin.md)。

一旦您熟悉了API，您可以查看[插件API参考](https://joplinapp.org/api/references/plugin_api/classes/joplin.html)，了解每个支持功能的详细文档。 