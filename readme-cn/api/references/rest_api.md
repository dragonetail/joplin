---
sidebar_position: 2
---

# Joplin数据API

当剪藏服务器运行时，此API可用。它通过REST API提供对笔记、笔记本、标签和其他Joplin对象的访问。即使剪藏服务器没有运行，插件也可以访问此API。

要使用它，您首先需要找出服务在哪个端口上运行。为此，打开Joplin中的Web剪藏器选项，如果服务正在运行，它应该告诉您运行在哪个端口上。通常它运行在端口**41184**上。如果您想以编程方式找到它，可以遵循这种算法：

```javascript
let port = null;
for (let portToTest = 41184; portToTest <= 41194; portToTest++) {
    const result = pingPort(portToTest); // 调用GET /ping
    if (result == 'JoplinClipperServer') {
        port = portToTest; // 找到端口
        break;
    }
}
```

## 授权

为防止未授权的应用程序访问API，必须对调用进行身份验证。为此，您必须为每个API调用提供一个令牌作为查询参数。您可以从Joplin桌面应用程序的Web剪藏器选项屏幕获取此令牌。

这是使用令牌的有效cURL调用示例：

```shell
curl http://localhost:41184/notes?token=ABCD123ABCD123ABCD123ABCD123ABCD123
```

在下面的文档中，不会每次都指定令牌，但您需要包含它。

如果需要，您也可以[以编程方式请求令牌](https://github.com/laurent22/joplin/blob/dev/readme/dev/spec/clipper_auth.md)

## 使用API

除非另有说明，所有调用都接收和发送**JSON数据**。例如创建一个新笔记：

```shell
curl --data '{ "title": "我的笔记", "body": "用**Markdown**写的一些笔记"}' http://localhost:41184/notes
```

在下面的文档中，调用可能包含特殊参数，如:id或:note_id。您会用项目ID或笔记ID替换它。

例如，对于端点`DELETE /tags/:id/notes/:note_id`，要从ID为"EFGH789"的笔记中删除ID为"ABCD1234"的标签，您可以运行例如：

```shell
curl -X DELETE http://localhost:41184/tags/ABCD1234/notes/EFGH789
```

API支持的四个动词如下：

* **GET**：检索项目（笔记、笔记本等）。
* **POST**：创建新项目。通常大多数项目属性是可选的。如果您省略任何属性，将使用默认值。
* **PUT**：更新项目。请注意，在传统的REST API中，PUT用于完全替换项目，但在此API中，它只会替换提供的属性。例如，如果您PUT {"title": "我的新标题"}，只有"title"属性会被更改。其他属性将保持不变（它们不会被清除或更改）。
* **DELETE**：删除项目。

## 过滤数据

您可以使用`fields=`查询参数更改API将返回的字段，该参数接受逗号分隔的字段列表。例如，要获取笔记的经度和纬度，请使用：

```shell
curl http://localhost:41184/notes/ABCD123?fields=longitude,latitude
```

要只获取所有标签的ID：

```shell
curl http://localhost:41184/tags?fields=id
```

默认情况下，API结果将包含以下字段：**id**、**parent_id**、**title**

## 分页

所有返回多个结果的API调用都将进行分页，并将返回以下结构：

键 | 是否总是存在？ | 描述
--- | --- | ---
`items` | 是 | 您请求的项目数组。
`has_more` | 是 | 如果为`true`，则此页面之后还有更多项目。如果为`false`，则表示您已到达数据集的末尾。

您可以使用`order_by`和`order_dir`查询参数指定结果的排序方式，使用`page`参数（从1开始并默认为1）指定要检索的页面。您可以使用`limit`参数（最大为100项）指定要返回的项目数。

例如，以下调用将启动一个请求，按"updated_time"升序每次获取10个笔记：

```shell
curl http://localhost:41184/notes?order_by=updated_time&order_dir=ASC&limit=10
```

这将返回一个如下结果：

```json
{ "items": [ /* 10个笔记 */ ], "has_more": true }
```

然后您将使用此查询继续获取结果：

```shell
curl http://localhost:41184/notes?order_by=updated_time&order_dir=ASC&limit=10&page=2
```

最终您将获得不包含"has_more"参数的结果，此时您已经检索了所有结果。

作为示例，下面的伪代码可用于获取所有笔记：

```javascript

async function fetchJson(url) {
	return (await fetch(url)).json();
}

async function fetchAllNotes() {
	let pageNum = 1;
	do {
		const response = await fetchJson((http://localhost:41184/notes?page=' + pageNum++);
		console.info('Printing notes:', response.items);
	} while (response.has_more)
}
```

## 错误处理

如果出现错误，将返回HTTP状态码>=400以及提供有关错误的更多信息的JSON对象。JSON对象的格式为`{ "error": "错误描述" }`。

## 关于属性类型

* 文本是UTF-8编码。
* 所有日期/时间都是毫秒级的Unix时间戳。
* 布尔值是整数值0或1。

## 测试服务是否可用

调用**GET /ping**以检查服务是否可用。如果可用，它应该返回"JoplinClipperServer"。

## 搜索

调用**GET /search?query=YOUR_QUERY**来搜索笔记。此端点支持`field`参数，建议使用该参数，以便您只获取所需的数据。查询语法如主文档所述：https://joplinapp.org/help/apps/search

要检索非笔记项目，如笔记本或标签，添加一个`type`参数并将其设置为所需的[项目类型名称](#项目类型ID)。在这种情况下，不会使用全文搜索 - 而是一个简单的不区分大小写的搜索。您也可以使用`*`作为通配符。这对于例如按标题检索笔记本或标签非常方便。

例如，检索名为`recipes`的笔记本：**GET /search?query=recipes&type=folder**

检索所有以`project-`开头的标签：**GET /search?query=project-*&type=tag**

## 项目类型ID

在您从API检索的某些对象中可能会引用项目类型ID。以下是名称和ID之间的对应关系：

名称 | 值
---- | -----
note | 1   
folder | 2   
setting | 3   
resource | 4   
tag | 5   
note_tag | 6   
search | 7   
alarm | 8   
master_key | 9   
item_change | 10   
note_resource | 11   
resource_local_state | 12   
revision | 13   
migration | 14   
smart_filter | 15   
command | 16   

## 笔记

### 属性

| 名称  | 类型  | 描述 |
| ----- | ----- | ----- |
| id    | 文本  |       |
| parent_id | 文本  | 包含此笔记的笔记本的ID。更改此ID可将笔记移动到不同的笔记本。 |
| title | 文本  | 笔记标题。 |
| body  | 文本  | 笔记正文，采用Markdown格式。也可能包含HTML。 |
| created_time | 整数   | 笔记创建时间。 |
| updated_time | 整数   | 笔记最后更新时间。 |
| is_conflict | 整数   | 表明笔记是否为冲突笔记。 |
| latitude | 数字 |       |
| longitude | 数字 |       |
| altitude | 数字 |       |
| author | 文本  |       |
| source_url | 文本  | 笔记来源的完整URL。 |
| is_todo | 整数   | 表明此笔记是否为待办事项。 |
| todo_due | 整数   | 待办事项的截止日期。将在该日期触发提醒。 |
| todo_completed | 整数   | 表明待办事项是否已完成。这是一个毫秒级的时间戳。 |
| source | 文本  |       |
| source_application | 文本  |       |
| application_data | 文本  |       |
| order | 数字 |       |
| user_created_time | 整数   | 笔记创建时间。它可能与created_time不同，因为它可以由用户手动设置。 |
| user_updated_time | 整数   | 笔记最后更新时间。它可能与updated_time不同，因为它可以由用户手动设置。 |
| encryption_cipher_text | 文本  |       |
| encryption_applied | 整数   |       |
| markup_language | 整数   |       |
| is_shared | 整数   |       |
| share_id | 文本  |       |
| conflict_original_id | 文本  |       |
| master_key_id | 文本  |       |
| user_data | 文本  |       |
| deleted_time | 整数   |       |
| body_html | 文本  | 以HTML格式的笔记正文 |
| base_url | 文本  | 如果提供了`body_html`并包含相对URL，也请提供`base_url`参数，以便所有URL都能转换为绝对URL。基本URL基本上是HTML的获取位置，减去查询部分（'?'之后的所有内容）。例如，如果原始页面是`https://stackoverflow.com/search?q=%5Bjava%5D+test`，那么基本URL是`https://stackoverflow.com/search`。 |
| image_data_url | 文本  | 要附加到笔记的图像，采用[Data URL](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)格式。 |
| crop_rect | 文本  | 如果提供了图像，您还可以指定一个可选的矩形，用于裁剪图像。格式为`{ x: x, y: y, width: width, height: height }` |

### GET /notes

获取所有笔记

默认情况下，此调用将返回所有笔记**除了**回收站文件夹中的笔记和任何冲突笔记。要包括这些内容，您可以将`include_deleted=1`和`include_conflicts=1`指定为查询参数。

### GET /notes/:id

获取ID为:id的笔记

### GET /notes/:id/tags

获取附加到此笔记的所有标签。

### GET /notes/:id/resources

获取附加到此笔记的所有资源。

### POST /notes

创建一个新笔记

您可以通过设置`body`参数以Markdown形式指定笔记正文，或通过设置`body_html`以HTML形式指定。

示例：

* 从一些Markdown文本创建笔记

```shell
curl --data '{ "title": "我的笔记", "body": "用**Markdown**写的一些笔记"}' http://127.0.0.1:41184/notes
```

* 从一些HTML创建笔记

```shell
curl --data '{ "title": "我的笔记", "body_html": "用<b>HTML</b>写的一些笔记"}' http://127.0.0.1:41184/notes
```

* 创建笔记并附加图像：

```shell
curl --data '{ "title": "图像测试", "body": "这是Joplin图标:", "image_data_url": "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAgAAAAICAIAAABLbSncAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAANZJREFUeNoAyAA3/wFwtO3K6gUB/vz2+Prw9fj/+/r+/wBZKAAExOgF4/MC9ff+MRH6Ui4E+/0Bqc/zutj6AgT+/Pz7+vv7++nu82c4DlMqCvLs8goA/gL8/fz09fb59vXa6vzZ6vjT5fbn6voD/fwC8vX4UiT9Zi//APHyAP8ACgUBAPv5APz7BPj2+DIaC2o3E+3o6ywaC5fT6gD6/QD9/QEVf9kD+/dcLQgJA/7v8vqfwOf18wA1IAIEVycAyt//v9XvAPv7APz8LhoIAPz9Ri4OAgwARgx4W/6fVeEAAAAASUVORK5CYII="}' http://127.0.0.1:41184/notes
```

#### 创建具有特定ID的笔记

当创建新笔记时，会自动分配一个新的唯一ID，因此**通常不需要设置ID**。然而，如果由于某种原因您想要设置它，可以将其作为`id`属性提供。它需要是**32个字符长的十六进制字符串**。**请确保它是唯一的**，例如通过使用您编程语言中可用的GUID函数生成它。

```shell
curl --data '{ "id": "00a87474082744c1a8515da6aa5792d2", "title": "具有自定义ID的笔记"}' http://127.0.0.1:41184/notes
```

### PUT /notes/:id

设置ID为:id的笔记的属性

### DELETE /notes/:id

删除ID为:id的笔记

默认情况下，笔记将被移动**到回收站**。要永久删除它，添加查询参数`permanent=1`

## 文件夹

这实际上是笔记本。在内部，笔记本被称为"文件夹"。

### 属性

| 名称  | 类型  | 描述 |
| ----- | ----- | ----- |
| id    | 文本  |       |
| title | 文本  | 文件夹标题。 |
| created_time | 整数   | 文件夹创建时间。 |
| updated_time | 整数   | 文件夹最后更新时间。 |
| user_created_time | 整数   | 文件夹创建时间。它可能与created_time不同，因为它可以由用户手动设置。 |
| user_updated_time | 整数   | 文件夹最后更新时间。它可能与updated_time不同，因为它可以由用户手动设置。 |
| encryption_cipher_text | 文本  |       |
| encryption_applied | 整数   |       |
| parent_id | 文本  |       |
| is_shared | 整数   |       |
| share_id | 文本  |       |
| master_key_id | 文本  |       |
| icon  | 文本  |       |
| user_data | 文本  |       |
| deleted_time | 整数   |       |

### GET /folders

获取所有文件夹

文件夹以树形结构返回。如果笔记本有子笔记本，它们将位于`children`键下。

### GET /folders/:id

获取ID为:id的文件夹

### GET /folders/:id/notes

获取此文件夹内的所有笔记。

### POST /folders

创建一个新文件夹

### PUT /folders/:id

设置ID为:id的文件夹的属性

### DELETE /folders/:id

删除ID为:id的文件夹

默认情况下，文件夹将被移动**到回收站**。要永久删除它，添加查询参数`permanent=1`

## 资源

### 属性

| 名称  | 类型  | 描述 |
| ----- | ----- | ----- |
| id    | 文本  |       |
| title | 文本  | 资源标题。 |
| mime  | 文本  |       |
| filename | 文本  |       |
| created_time | 整数   | 资源创建时间。 |
| updated_time | 整数   | 资源最后更新时间。 |
| user_created_time | 整数   | 资源创建时间。它可能与created_time不同，因为它可以由用户手动设置。 |
| user_updated_time | 整数   | 资源最后更新时间。它可能与updated_time不同，因为它可以由用户手动设置。 |
| file_extension | 文本  |       |
| encryption_cipher_text | 文本  |       |
| encryption_applied | 整数   |       |
| encryption_blob_encrypted | 整数   |       |
| size  | 整数   |       |
| is_shared | 整数   |       |
| share_id | 文本  |       |
| master_key_id | 文本  |       |
| user_data | 文本  |       |
| blob_updated_time | 整数   |       |
| ocr_text | 文本  |       |
| ocr_details | 文本  |       |
| ocr_status | 整数   |       |
| ocr_error | 文本  |       |

### GET /resources

获取所有资源

### GET /resources/:id

获取ID为:id的资源

### GET /resources/:id/file

获取与此资源关联的实际文件。

### GET /resources/:id/notes

获取与资源关联的笔记(ID)。

### POST /resources

创建一个新资源

创建新资源有些特别，因为您还需要上传文件。与其他API调用不同，此调用必须具有"multipart/form-data"Content-Type。文件数据必须传递给"data"表单字段，其他属性传递给"props"表单字段。使用cURL的有效调用示例如下：

```shell
curl -F 'data=@/path/to/file.jpg' -F 'props={"title":"我的资源标题"}' http://localhost:41184/resources
```

要**更新**资源内容，您可以使用相同的参数进行PUT请求：

```shell
curl -X PUT -F 'data=@/path/to/file.jpg' -F 'props={"title":"我的修改后的标题"}' http://localhost:41184/resources/8fe1417d7b184324bf6b0122b76c4696
```

"data"字段是必需的，而"props"字段不是。如果未指定，将使用默认值。

或者，如果您只需要更新资源属性(标题等)，而不更改内容，可以进行常规PUT请求：

```shell
curl -X PUT --data '{"title": "我的新标题"}' http://localhost:41184/resources/8fe1417d7b184324bf6b0122b76c4696
```

**从插件**创建资源的语法也有些特别：

```javascript
	await joplin.data.post(
		["resources"],
		null,
		{ title: "test.jpg" }, // 资源元数据
		[
			{
				path: "/path/to/test.jpg", // 实际文件
			},
		]
	);
```
### PUT /resources/:id

设置ID为:id的资源的属性

您也可以通过指定文件来更新文件数据(参见`POST /resources`示例)。

### DELETE /resources/:id

删除ID为:id的资源

## 标签

### 属性

| 名称  | 类型  | 描述 |
| ----- | ----- | ----- |
| id    | 文本  |       |
| title | 文本  | 标签标题。 |
| created_time | 整数   | 标签创建时间。 |
| updated_time | 整数   | 标签最后更新时间。 |
| user_created_time | 整数   | 标签创建时间。它可能与created_time不同，因为它可以由用户手动设置。 |
| user_updated_time | 整数   | 标签最后更新时间。它可能与updated_time不同，因为它可以由用户手动设置。 |
| encryption_cipher_text | 文本  |       |
| encryption_applied | 整数   |       |
| is_shared | 整数   |       |
| parent_id | 文本  |       |
| user_data | 文本  |       |

### GET /tags

获取所有标签

### GET /tags/:id

获取ID为:id的标签

### GET /tags/:id/notes

获取具有此标签的所有笔记。

### POST /tags

创建一个新标签

### POST /tags/:id/notes

向此端点发布笔记以将标签添加到笔记。笔记数据至少必须包含ID属性(所有其他属性将被忽略)。

### PUT /tags/:id

设置ID为:id的标签的属性

### DELETE /tags/:id

删除ID为:id的标签

### DELETE /tags/:id/notes/:note_id

从笔记中移除标签。

## 修订版本

### 属性

| 名称  | 类型  | 描述 |
| ----- | ----- | ----- |
| id    | 文本  |       |
| parent_id | 文本  |       |
| item_type | 整数   |       |
| item_id | 文本  |       |
| item_updated_time | 整数   |       |
| title_diff | 文本  |       |
| body_diff | 文本  |       |
| metadata_diff | 文本  |       |
| encryption_cipher_text | 文本  |       |
| encryption_applied | 整数   |       |
| updated_time | 整数   |       |
| created_time | 整数   |       |

### GET /revisions

获取所有修订版本

### GET /revisions/:id

获取ID为:id的修订版本

### POST /revisions

创建一个新修订版本

### PUT /revisions/:id

设置ID为:id的修订版本的属性

### DELETE /revisions/:id

删除ID为:id的修订版本

## 事件

此端点可用于检索最新的笔记更改。目前只跟踪笔记更改。

### 属性

| 名称  | 类型  | 描述 |
| ----- | ----- | ----- |
| id    | 整数   |       |
| item_type | 整数   | 项目类型(参见上表中的项目类型列表) |
| item_id | 文本  | 项目ID |
| type  | 整数   | 更改类型 - 1(创建)、2(更新)或3(删除) |
| created_time | 整数   | 事件生成时间 |
| source | 整数   | 未使用 |
| before_change_item | 文本  | 未使用 |

### GET /events

返回最近事件的分页列表。应提供`cursor`属性，告知从何时开始返回事件。API将返回一个`cursor`属性，以告知从何处继续检索事件，以及一个`has_more`(告知是否可以检索更多更改)和`items`属性，其中将包含事件列表。事件保存长达90天。

如果未提供`cursor`属性，API将使用最新的更改ID进行响应。这可用于稍后检索未来事件。

结果是分页的，因此您可能需要多次调用才能检索所有事件。使用`has_more`属性来了解是否可以检索更多内容。

### GET /events/:id

返回具有给定ID的事件。