# 统计数据

为了改进应用程序，我们收集了关于Joplin使用情况的基本和匿名统计数据。这些统计数据包括如下可能敏感的信息：笔记计数、标签计数，等等，但不包括笔记内容、文件名，或者任何其他可能会泄露有关存储在Joplin中的信息的内容。

统计信息仅在桌面应用上收集。

如果您不希望分享此信息，您可以在设置中禁用统计收集。

## 目的

收集此数据的目的是：

- 优先考虑开发哪些功能。例如，如果几乎没有人使用同步功能，开发它的重要性可能会降低，反之亦然，如果很多人使用它，在改进它方面可能会有更多资源投入。
- 检测错误或性能问题。例如，如果有一个变量应该始终为正数，但我们看到10%的用户上报为负数，那么我们可以调查此错误。

## 范例数据

```
{
  "appId": "b047e64f8b1c4a0ab608f48566c26724", // 最初安装时创建的随机值
  "appType": "desktop",
  "timestamp": 1661601607384,
  "version": "2.9.1",
  "os": "darwin",
  "syncInfo": {
    "isEnabled": false, // 是否启用了同步
    "target": 0, // 同步到的位置
    "itemCount": 0 // 同步操作中的项目数量
  },
  "counters": {
    "folderCount": 17, // 笔记本数量
    "noteCount": 1321, // 笔记数量
    "tagCount": 29, // 标签数量
    "resourceCount": 168, // 附件文件数量
    "noteContentLength": 679487, // 所有笔记的总长度（以字节为单位）
    "masterKeyCount": 0, // 主密钥数量
    "hasHostedImage": true, // 如果有笔记包含一个托管的图片
    "hasCustomCss": true, // 如果设置了自定义CSS
    "customCssLength": 863, // 用户CSS的长度
    "activeFolderId": "19b8a897d71a46ccb3584c8d9a6770a9", // 当前选择的文件夹的ID
    "foldersWithEmojis": 4 // 带有emoji标题的文件夹数量
  },
  "settings": {
    "locale": "en_GB", // 应用程序语言
    "theme": "light", // 应用程序主题
    "spellcheckEnabled": true, // 是否启用拼写检查
    "spellcheckLanguages": [
      "en-GB",
      "fr-FR",
      "en-US"
    ],
    "activeBrand": "Joplin", // 设置的品牌  
    "newNotebookTemplate": false, // 是否设置了笔记本模板
    "newNoteTemplate": false, // 是否设置了笔记模板
    "newTodoTemplate": false, // 是否设置了待办事项模板
    "plugins": 5, // 已安装的插件数量
    "revisionService": false // 是否启用历史版本服务
  },
  "clipperInfo": {
    "isEnabled": false, // 是否启用剪藏器
    "clipperServerPort": 41184 // 剪藏器运行的端口
  },
  "activeAccount": 0 // 通过新统一登录页面登录的账户类型
}
```

## 使用的技术

- Joplin使用[Amplitude SDK](https://amplitude.com/)将统计数据发送到[Amplitude服务](https://amplitude.com/)。
- 数据被聚合和显示在一个仪表板上，这样可以更容易地发现趋势和相关性（例如，使用哪种同步方法的用户遇到的同步问题最少？使用哪种同步方法的用户拥有最多的笔记？）

## 查看您的数据

您可以使用[Amplitude请求表单](https://amplitude.com/data-subject-request)查看与您的userId（也称为appId）相关的所有数据，该ID可以在桌面应用程序设置中的"统计和遥测"部分找到。

## 替代选项

我们也考虑了其他选项：

- **自己的后端和仪表板**：自行构建整个数据收集后端是一项重大的开发工作，并且还需要维护一个单独的数据库，所有这些都是额外的工作。
- **开放式应用分析**：这些工具对开源项目有限制。例如，Plausible的开源项目计划是每月500,000次页面浏览量，而对于在两个平台上为几个版本收集统计数据，我们经常超过这个上限（每月2,000,000+事件）。

## 删除您的数据

当您使用Joplin时，产生的数据存储在您的设备上。如果设置启用了遥测功能，上述某些数据也会被传输到Amplitude处理。要删除您的Amplitude用户数据，请按照[Amplitude删除数据请求说明](https://amplitude.com/dsar)进行操作。同样，你可以在Joplin桌面应用程序的"统计和遥测"设置中找到您的用户ID。 