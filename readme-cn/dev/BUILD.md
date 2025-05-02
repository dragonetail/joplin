# 构建应用程序

Joplin源代码托管在[单体仓库](https://en.wikipedia.org/wiki/Monorepo)上，并使用Yarn工作区管理（以及Lerna用于发布软件包）。

主要子包的列表如下：

包名 | 描述
--- | ---
app-cli | 命令行应用程序
app-clipper | 网页剪藏器
app-desktop | 桌面应用程序
app-mobile | 移动应用程序
lib | 所有应用程序共享的核心库。它处理同步、加密、导入/导出、数据库以及几乎所有应用程序业务逻辑
renderer | Joplin Markdown和HTML渲染器
tools | 用于构建应用程序和其他任务的工具

还有一些现有包的分支，它们的名称以"fork-*"开头。

## 所需依赖项

所有必需的依赖项都列在项目根目录的[devbox.json](https://github.com/laurent22/joplin/blob/dev/devbox.json)文件中。您可以根据该列表手动安装它们，或者在Linux或MacOS上使用以下命令自动安装：

```sh
devbox shell
```

如果您还没有安装devbox，请[按照这些说明](https://www.jetify.com/docs/devbox/quickstart/)进行操作。

如果您在开发`onenote-converter`包，您需要安装[Rust工具链](https://rustup.rs/)。

## 构建

确保项目目录的路径不包含空格，否则构建可能会失败。

在执行任何其他操作之前，从项目根目录运行：

	yarn install

然后您可以测试各种应用程序：

## 测试桌面应用程序

	cd packages/app-desktop
	yarn start

在Windows中使用常规命令提示符进行开发。我们[不建议为此使用WSL](https://github.com/laurent22/joplin/blob/dev/readme/dev/build_troubleshooting.md#other-issues)，并且我们不支持这种用例。

## 测试终端应用程序

	cd packages/app-cli
	yarn start

## 测试移动应用程序

首先，您需要设置React Native来构建包含原生代码的项目。为此，请按照[设置开发环境](https://reactnative.dev/docs/environment-setup)教程中的说明进行操作，查看"React Native CLI快速入门"选项卡。

### Android

运行以下命令在模拟器上构建和安装应用程序：

	cd packages/app-mobile/android
	./gradlew installDebug # 在Windows上使用gradlew.bat installDebug

### iOS

在iOS上，您需要运行`pod install`，这在构建过程中不会自动完成（因为耗时太长）。您有两个选择：

- 使用`RUN_POD_INSTALL=1 yarn install`构建应用程序
- 或者从`packages/app-mobile/ios`手动运行`pod install`

完成后，在XCode上打开文件`ios/Joplin.xcworkspace`，然后从那里运行应用程序。

通常**bundler**应该随应用程序自动启动。如果没有启动，请从`packages/app-mobile`运行`yarn start`。

### Web版

要在Web浏览器中运行移动应用程序，请执行以下操作：

	cd packages/app-mobile
	yarn serve-web

上面的`yarn serve-web`在端口`8088`上启动一个开发服务器。当对源文件进行更改时，Web应用程序的构建版本会自动重新加载整个页面。

要在更改时重新加载单个组件（热重载），请使用以下命令提供服务：

	yarn serve-web-hot-reload

要创建发布版本，请运行`yarn web`。构建输出将存储在`packages/app-mobile/web/dist`中。

与iOS和Android构建一样，有必要将TypeScript编译为JS。请参阅下面的"监视文件"。

## 构建剪藏器

	cd packages/app-clipper/popup
	npm run watch # 监视更改

要测试扩展，请参考每个浏览器的相关页面：[Firefox](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/Your_first_WebExtension#Trying_it_out) / [Chrome](https://developer.chrome.com/docs/extensions/mv3/getstarted/)。请注意，开发模式下的扩展只会连接到桌面应用程序的开发实例（反之亦然）。

## 监视文件

要对应用程序进行更改，您需要重新构建您更改的任何TypeScript文件。最简单的方法是从项目根目录监视更改。只需运行此命令，它应该会处理其余部分：

	yarn watch

运行`yarn tsc`会有相同的效果，但不会监视文件变化。

**移动特定说明**：如果您对笔记编辑器、查看器或其他WebView内容进行更改，请从`packages/app-mobile`运行`yarn watchInjectedJs`，以在更改时重新构建WebView JavaScript文件。

## 使用附加参数运行应用程序

您可以在运行桌面或CLI应用程序时指定附加参数。为此，请在`yarn start`命令后添加`--`，后跟您的标志。例如：

	yarn start --debug

## TypeScript

该应用程序最初是用JavaScript编写的，但它已逐渐迁移到[TypeScript](https://www.typescriptlang.org/)。新的类和文件应该用TypeScript编写。所有编译的文件都生成在.ts或.tsx文件旁边。例如，如果有一个文件"lib/MyClass.ts"，则会在其旁边生成一个"lib/MyClass.js"。这样实现是因为它需要对现有JavaScript代码库进行最小的更改以集成TypeScript。

## 故障排除

请阅读[构建故障排除文档](https://github.com/laurent22/joplin/blob/dev/readme/dev/build_troubleshooting.md)，了解关于如何使构建工作的各种提示。 