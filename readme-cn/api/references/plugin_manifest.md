# 插件清单

清单文件是一个描述插件各种属性的JSON文件。如果您使用Yeoman生成器，它应该会根据您提供的答案自动生成。支持的属性有：

名称 | 类型 | 是否必需？ | 描述
--- | --- | --- | ---
`manifest_version` | 数字 | **是** | 目前应始终为"1"。
`name` | 字符串 | **是** | 插件名称。应该是用户友好的字符串，因为它将显示在UI中。
`version` | 字符串 | **是** | 版本号，如"1.0.0"。
`app_min_version` | 字符串 | **是** | 插件兼容的Joplin最低版本。通常应该是您用于开发插件的版本。
`app_min_version_mobile` | 字符串 | 否 | 移动平台上Joplin的最低版本，如果与`app_min_version`不同。
`platforms` | 字符串[] | 否 | 插件支持的平台列表。例如，`["desktop", "mobile"]`。
`description` | 字符串 | 否 | 插件的详细描述。
`author` | 字符串 | 否 | 插件作者名称。
`keywords` | 字符串[] | 否 | 与插件关联的关键词。它们用于搜索。
`homepage_url` | 字符串 | 否 | 插件的主页URL。它也可以是，例如，指向GitHub仓库的链接。
`repository_url` | 字符串 | 否 | 托管插件源代码的仓库URL。
`categories` | 字符串[] | 否 | 描述插件功能的[类别](#类别)。
`screenshots` | Image[] | 否  | [截图](#截图)用于在Joplin插件网站上展示。
`icons` | Icons | 否 | 如果未提供[图标](#图标)，默认情况下将使用标准插件图标。您应该至少提供一个主图标，理想情况下大小为48x48像素。这是将在各种插件页面中使用的图标。但是，您可以提供任何大小的图标，Joplin将尝试找到最适合在不同组件中显示的图标。只允许PNG图标。
`promo_tile` | Image | 否 | [宣传图块](#宣传图块)是一个可选的图像，用于在Joplin插件网站上推广您的插件。

## 平台

可以包含`"desktop"`和/或`"mobile"`的列表。如果未指定，对于大多数插件，默认为`[ "desktop" ]`。

## 类别

| 类别 | 描述 |
| --- | --- |
| appearance | 处理应用程序某些元素的外观。例如行号、布局等。 |
| developer tools | 为开发者构建的工具。 |
| editor | 增强笔记编辑器。 |
| files | 处理文件。例如导入、导出、备份等。 |
| integrations | 集成第三方服务或应用程序。 |
| personal knowledge management | 管理和组织笔记。 |
| productivity | 使Joplin使用更加高效。 |
| search | 增强应用内搜索功能。 |
| tags | 处理笔记标签。 |
| themes | 更改应用程序主题。 |
| viewer | 增强笔记的渲染效果。 |

## 截图

| 属性 | 描述 |
| --- | --- |
| src | 指向截图的路径或URL。如果是路径，`src`应该相对于仓库根目录（例如`screenshots/a.png`）。 |
| label | 图像的描述。此标签将被屏幕阅读器使用，或在图像无法加载时显示。 |

**注意**：如果`src`是路径而不是URL，则`repository_url`或`homepage_url`必须指向GitHub仓库，才能在Joplin插件网站上显示截图。参见[相关问题](https://github.com/joplin/website-plugin-discovery/issues/35)。

## 图标

| 属性 | 描述 |
| --- | --- |
| 16 | PNG图标的路径。 |
| 32 | PNG图标的路径。 |
| 48 | PNG图标的路径。 |
| 128 | PNG图标的路径。 |

注意：所有路径都应该相对于仓库根目录。

## 宣传图块

这是一个可选的图像，显示在Joplin插件网站的主页上。这是一个机会，通过使用吸引人的图像来推广您的插件。开始制作宣传图块的好方法是显示您的图标或徽标以及插件名称。看看Chrome网上应用店[有很多好的宣传图块示例](https://chromewebstore.google.com/category/extensions/lifestyle/social)。

如果没有提供宣传图块，将显示您的插件图标。

| 属性 | 描述 |
| --- | --- |
| src | 指向截图的路径或URL。它必须是**440 x 280图像**的JPEG或PNG（无alpha通道）。如果是路径，`src`应该相对于仓库根目录（例如`images/promo_tile.png`）。 |
| label | 图像的描述。此标签将被屏幕阅读器使用，或在图像无法加载时显示。 |

## 清单示例

```json
{
    "manifest_version": 1,
    "name": "Joplin简单插件",
    "description": "用于测试加载和运行插件",
    "version": "1.0.0",
    "author": "张三",
    "app_min_version": "1.4",
    "app_min_version_mobile": "3.0.3",
    "platforms": ["mobile", "desktop"],
    "homepage_url": "https://joplinapp.org",
    "screenshots": [
      {
        "src": "images/screenshot.png",
        "label": "插件使用示例"
      },
      {
        "src": "https://example.com/images/screenshot.png",
        "label": "插件加载屏幕"
      }
    ],
    "icons": {
      "16": "images/icon16.png",
      "32": "images/icon32.png",
      "48": "images/icon48.png",
      "128": "images/icon128.png"
    },
    "promo_tile": {
      "src": "images/promo_tile.png",
      "label": "清晰背景上的插件徽标"
    }
}
``` 