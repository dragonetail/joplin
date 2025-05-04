# 使用技巧

前几个笔记应该已经让您了解了Joplin的主要功能，但它能做的还有更多。请参阅下面的一些功能以及如何获取更多应用使用帮助：

## 网页剪辑器

![](./WebClipper.png)

网页剪辑器是一个浏览器扩展，允许您从浏览器保存网页和截图。要开始使用它，请打开Joplin桌面应用程序，进入网页剪辑器选项并按照说明进行操作。

在官方网站上查看更多信息：https://joplinapp.org/help/apps/clipper

## 插件

Joplin支持许多插件，允许您向应用程序添加新功能，例如标签页、笔记的目录、管理收藏笔记的方式等等。要添加插件，请转到配置屏幕中的"插件"部分。从那里，您可以搜索和安装插件，也可以搜索或更新插件。

## 附件

任何类型的文件都可以附加到笔记中。在Markdown中，这些文件的链接表示为ID。在笔记查看器中，如果这些文件是图像，将被显示出来；如果是其他文件（PDF、文本文件等），它们将显示为链接。点击此链接将在默认应用程序中打开文件。

可以通过点击"附加文件"或直接在编辑器中粘贴（使用`Ctrl+V`或`Cmd+V`）图像，或通过拖放图像来附加图像。

关于附件的更多信息：https://joplinapp.org/help/apps/attachments

## 搜索

Joplin支持高级搜索查询，这些查询在官方网站上有完整的文档：https://joplinapp.org/help/apps/search

## 提醒

可以将提醒与任何待办事项关联。它将在给定时间通过显示通知来触发。要使用此功能，请参阅文档：https://joplinapp.org/help/apps/notifications

## Markdown高级技巧

Joplin使用并渲染[Github风格的Markdown](https://joplinapp.org/help/apps/markdown)，并有一些变体和添加。

例如，支持表格：

| 表格          | 是            | 酷   |
| ------------- |:-------------:| -----:|
| 第3列         | 右对齐        | $1600 |
| 第2列         | 居中对齐      |   $12 |
| 斑马条纹      | 很整洁        |    $1 |

您还可以创建复选框列表。这些复选框可以直接在查看器中勾选，或通过在里面添加"x"：

- [ ] 牛奶
- [ ] 鸡蛋
- [x] 啤酒

可以使用[KaTeX符号](https://khan.github.io/KaTeX/)添加数学表达式：

$$
f(x) = \int_{-\infty}^\infty
    \hat f(\xi)\,e^{2 \pi i \xi x}
    \,d\xi
$$

还可以使用各种其他技巧，例如使用HTML或自定义CSS。有关更多信息，请参阅Markdown文档 - https://joplinapp.org/help/apps/markdown

## 社区和进一步帮助

- 有关Joplin的一般讨论、用户支持、软件开发问题以及讨论新功能，请前往[Joplin论坛](https://discourse.joplinapp.org/)。可以使用GitHub账户登录。
- 最新消息发布在[Patreon页面上](https://www.patreon.com/joplin)。
- 对于错误报告和功能请求，请前往[GitHub问题跟踪器](https://github.com/laurent22/joplin/issues)。

## 捐赠

对Joplin的捐赠支持项目的开发。开发高质量的应用程序主要需要时间，但也有一些费用，例如签署应用程序的数字证书、应用商店费用、托管等。最重要的是，您的捐赠将使保持当前的开发标准成为可能。

请参阅[捐赠页面](https://joplinapp.org/donate/)了解如何支持Joplin的开发信息。 