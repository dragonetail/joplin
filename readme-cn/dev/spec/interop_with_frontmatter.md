# 带前置元数据的Markdown导出器/导入器

此导出器/导入器基于MD导出器/导入器构建。它的功能完全相同，但包含一个YAML前置元数据块，其中包含笔记的元数据。

YAML前置元数据简单地表示为`---`分隔符之间的YAML块。下面是一个示例：

```
---
title: Joplin互操作性
created: 1970-01-01 00:00Z
tags:
  - export
  - import
---

笔记正文
```

`---`分隔符和笔记正文之间应该有一个空行。之后的任何空行将被视为笔记正文的一部分。导入笔记时，如果在`---`分隔符和笔记正文之间没有空行，那么`---`之后的所有内容都将被视为笔记正文。

## 支持的元数据字段

以下所有字段都受导出器和导入器支持：

- `title`：笔记标题
- `updated`：笔记最后更新时间（对应于`user_updated_time`）
- `created`：笔记创建时间（对应于`user_created_time`）
- `source`：来自Web剪藏器的笔记的源URL
- `author`：作者姓名
- `latitude`：创建笔记的纬度
- `longitude`：创建笔记的经度
- `altitude`：创建笔记的海拔
- `completed?`：如果笔记是待办事项，则存在此字段，表示待办事项是否已完成
- `due`：如果笔记是待办事项，则存在此字段，表示笔记的截止日期（提醒时间）
- `tags`：所有关联标签名称的列表

### 导出器

导出器将导出数据库中持有值的所有上述字段。因此，`due`和`completed?`只会为"待办事项"笔记包含，`tags`只会为包含标签的笔记存在，等等。

### 导入器

导入器将导入与所有上述字段对应的元数据。缺失的数据将被填充，就像刚刚创建笔记一样。额外的字段将被忽略。

还有其他工具使用类似的YAML前置元数据块，特别是[pandoc](https://pandoc.org/MANUAL.html#extension-yaml_metadata_block)和[r-markdown](https://github.com/hao203/rmarkdown-YAML)。导入器尝试在可能的情况下提供与这些格式的兼容性。

## 日期
### 导出器

所有日期都以ISO 8601格式导出（根据RFC 3339，为了可读性将'T'替换为空格），使用UTC时区。

例如：`1970-01-01 00:00:00Z`

### 导入器

导入器对日期的处理更加灵活。它将处理带有或不带有时区的ISO 8601日期，如果未指定时区，将使用本地时间。如果指定了时区（Z表示法或+00:00表示法），则将使用该时区。如果格式不是ISO 8601，导入器将尝试根据用户配置的日期和时间首选项（工具 -> 选项 -> 常规或Joplin -> 首选项 -> 常规）进行读取。如果无法读取格式，导入器将回退到JavaScript的[Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)功能。

## 示例

以下是一系列示例，代表可能已由导出器导出并可由导入器导入的有效笔记。

```
---
title: 青蛙
source: https://en.wikipedia.org/wiki/Frog
created: 2021-05-01 16:40:00Z
updated: 2021-05-01 16:40:00Z
tags:
  - 参考
  - 酷
---

本文是关于两栖动物的。有关其他用途，请参阅[青蛙（消歧义）](https://en.wikipedia.org/wiki/Frog_%28disambiguation%29 "青蛙（消歧义）")。
...
```

```
---
title: 带回家的测验
created: 2021-05-01 16:40:00Z
updated: 2021-06-17 23:59:00Z
tags:
  - 学校
  - 数学
  - 家庭作业
completed?: no
due: 2021-06-18 08:00:00Z
---

**证明或给出以下陈述的反例：**

> 在三维空间和时间中，给定一个初始速度场，存在一个向量速度和一个标量压力场，它们都是平滑且全局定义的，可以解决纳维-斯托克斯方程。
```

```
---
title: 所有字段
updated: 2019-05-01 16:54:00Z
created: 2019-05-01 16:54:00Z
source: https://joplinapp.org
author: Joplin
latitude: 37.084021
longitude: -94.51350100
altitude: 0.0000
completed?: no
due: 2021-08-22 00:00:00Z
tags:
  - joplin
  - 笔记
  - 铅笔
---

所有这些元数据都可以导入/导出。
``` 