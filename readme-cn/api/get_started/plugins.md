# 插件开发入门

在本文中，您将学习构建和测试Joplin插件的基本步骤。

## 设置您的环境

首先，您需要设置您的环境：

- 确保您已安装[Node.js](https://nodejs.org/)和[git](https://git-scm.com)。
- 安装[Joplin](https://joplinapp.org/)

首先安装[Yeoman](https://yeoman.io/)和[Joplin插件生成器](https://github.com/laurent22/joplin/tree/dev/packages/generator-joplin)：
	
	npm install -g yo generator-joplin

然后，在您计划开发插件的目录中，运行：

	yo joplin

这将生成插件的基本脚手架。在它的根目录中，有一些配置文件，您通常不需要更改。然后`src/`目录将包含您的代码。默认情况下，项目使用TypeScript，但您也可以自由使用普通JavaScript - 最终项目在任何情况下都会编译为普通JS。

`src/`目录还包含一个[manifest.json](https://github.com/laurent22/joplin/blob/dev/readme/api/references/plugin_manifest.md)文件，其中包含在脚手架初始生成过程中设置的有关插件的各种信息，如其名称、主页URL等。您可以随时编辑它，但在发布后编辑它可能会导致用户必须再次下载它。

## 设置源代码控制

在您的插件目录中，运行：

	git init 

这将设置源代码控制。


## 在开发模式下运行Joplin

您应该在[开发模式](https://github.com/laurent22/joplin/blob/dev/readme/api/references/development_mode.md)下测试您的插件。这样做意味着Joplin将使用不同的配置文件运行，所以您可以尝试使用插件，而不会冒意外更改或删除数据的风险。

## 构建插件

从脚手架中，`src/index.ts`现在包含了Hello World插件的基本代码。

需要注意两点：
1. 它包含对[joplin.plugins.register](https://joplinapp.org/api/references/plugin_api/classes/joplinplugins.html#register)的调用。所有插件都调用此函数在应用程序中注册插件。
2. 一个`onStart()`事件处理方法，它在插件启动时被调用。

要尝试这个基本插件，请从项目根目录运行以下命令编译应用程序：

	npm run dist

这样做应该将所有文件编译到`dist/`目录中。这是Joplin将加载插件的地方。

## 安装插件
打开Joplin的**配置 > 插件**部分。在高级设置下，将插件路径添加到**开发插件**文本字段中。
这应该是您的主插件目录的路径，即`path/to/your/root/plugin/directory`。

## 测试插件，Hello World！
从命令行/终端重新启动开发应用程序，Joplin应该加载插件并执行其`onStart`处理程序。如果一切顺利，您应该会在插件控制台中看到测试消息："Hello world. Test plugin started!"。您还将能够在**设置 > 插件**中看到来自清单的信息。

## 下一步
太好了，您现在拥有了一个可工作插件的基础！

- 开始[插件教程](https://github.com/laurent22/joplin/blob/dev/readme/api/tutorials/toc_plugin.md)，了解如何使用插件API。
- 查看插件API支持什么，[插件API参考](https://joplinapp.org/api/references/plugin_api/classes/joplin.html)。
- 有关插件功能的想法，请参阅此讨论：https://discourse.joplinapp.org/t/any-suggestions-on-what-plugins-could-be-created/9479 