# Markdown指南

Markdown是一种简单的文本格式化方式，在任何设备上都看起来很棒。它不做任何花哨的操作，如更改字体大小、颜色或类型 — 只有基本功能，使用您已经知道的键盘符号。由于它是纯文本，它是一种简单的方式来编写笔记和文档，并且在需要时可以转换为富文本HTML文档。

Joplin桌面和移动应用程序可以同时显示Markdown文本和渲染的富文本文档。

Joplin遵循[CommonMark](https://spec.commonmark.org/)规范，并通过插件添加额外功能。

## 速查表

这是Markdown语法的快速摘要。

|     | Markdown | 渲染输出
| --- | --- | ---
| **标题1** | <pre># 标题1</pre> | <h1>标题1</h1>
| **标题2** | <pre>## 标题2</pre> | <h2>标题2</h2>
| **标题3** | <pre>### 标题3</pre> | <h3>标题3</h3>
| **粗体** | <pre>这是一些`**粗体文本**`</pre> | 这是一些<strong>粗体文本</strong>
| **斜体** | <pre>这是一些`*斜体文本*`</pre> | 这是一些<i>斜体文本</i>
| **引用块** | <pre>> 肯特。<br/>> 国王在哪里？<br/><br/>> 绅士。<br/>> 正在与<br/>> 躁动的元素抗争</pre> | <blockquote>肯特。<br/>国王在哪里？<br/><br/>绅士。<br/>正在与<br/>躁动的元素抗争</blockquote>
| **列表** | <pre>- 牛奶<br/>- 鸡蛋<br/>- 啤酒<br/>    - Desperados<br/>    - Heineken<br/>- 火腿</pre> | <ul><li>牛奶</li><li>鸡蛋</li><li>啤酒<ul><li>Desperados</li><li>Heineken</li></ul></li><li>火腿</li></ul>
| **有序列表** | <pre>1. 引言<br/>2. 主题<br/>    1. 第一个子主题<br/>    2. 第二个子主题<br/>3. 结论</pre> | <ol><li>引言</li><li>主题<ol><li>第一个子主题</li><li>第二个子主题</li></ol></li><li>结论</li></ol>
| **内联代码** | <pre>这是\`someJavaScript()\`</pre> | 这是`someJavaScript()`
| **代码块** | <pre>这里有一些JavaScript代码：<br><br>\`\`\`<br>function hello() {<br>    alert('hello');<br>}<br>\`\`\`<br><br>语言通常会自动检测，<br>但也可以指定：<br><br>\`\`\`sql<br>SELECT * FROM users;<br>DELETE FROM sessions;<br>\`\`\`</pre> | 这里有一些JavaScript代码：<br><br><pre>function hello() {<br>&nbsp;&nbsp;&nbsp;&nbsp;alert('hello');<br>}</pre><br>语言通常会自动检测，但也可以指定：<br><br><pre>SELECT * FROM users;<br>DELETE FROM sessions;</pre>
| **未格式化文本** | <pre>缩进一个制表符或4个空格<br>以获得未格式化文本。<br/><br/>    此文本将不会被格式化：<br><br>    Robert'); DROP TABLE students;--</pre> | 缩进一个制表符或4个空格以获得未格式化文本。<br><br><pre>此文本将不会被格式化：<br><br>Robert'); DROP TABLE students;--</pre>
| **链接** | <pre>这被检测为链接：<br><br>`https://joplinapp.org`<br><br>这是锚定文本内容的链接：<br><br>`[Joplin](https://joplinapp.org)`<br><br>这是一个带有标题的链接，<br>锚定文本内容：<br><br>`[Joplin](https://joplinapp.org "Joplin项目页面")`</pre> | 这被检测为链接：<br><br>https://joplinapp.org<br><br>这是锚定文本内容的链接：<br><br>[Joplin](https://joplinapp.org)<br><br>这是一个带有标题的链接，<br>锚定文本内容：<br><br>[Joplin](https://joplinapp.org "Joplin项目页面") (_提示：将鼠标悬停在链接上_)
| **图片** | <pre>`![Joplin图标](https://git.io/JenGk)`</pre> | ![Joplin图标](https://git.io/JenGk)
| **水平线** | <pre>一条线：<br>\*\*\*<br>另一条线：<br>\-\-\-</pre> | 一条线：<hr><br>另一条线：<br><hr>
| **表格** | [见下文](#表格) |

### 表格

表格使用管道符`|`和连字符`-`创建。这是一个Markdown表格：

	| 第一标题  | 第二标题 |
	| ------------- | ------------- |
	| 内容单元格  | 内容单元格  |
	| 内容单元格  | 内容单元格  |

渲染后如下：

| 第一标题  | 第二标题 |
| ------------- | ------------- |
| 内容单元格  | 内容单元格  |
| 内容单元格  | 内容单元格  |

注意，必须至少有3个破折号分隔每个标题单元格。

可以使用冒号来对齐列：

	| 表格        | 是           | 酷  |
	| ------------- |:-------------:| -----:|
	| 第3列      | 右对齐 | $1600 |
	| 第2列      | 居中      |   $12 |

渲染后如下：

| 表格        | 是           | 酷  |
| ------------- |:-------------:| -----:|
| 第3列      | 右对齐 | $1600 |
| 第2列      | 居中      |   $12 |

## Joplin额外功能

除了标准的Markdown语法外，Joplin还支持几个附加功能。

### 链接到其他笔记

您可以通过在URL中指定笔记的ID来创建到笔记的链接。例如：

	[链接到我的笔记](:/0b0d62d15e60409dac34f354b6e9e839)

由于获取笔记ID并不直观，每个应用程序都提供了创建此类链接的方法。在**桌面应用程序**中，将一个笔记拖放到另一个笔记中以创建链接。或右键单击笔记并选择"复制Markdown链接"。在**移动应用程序**中，打开笔记，并在右上角菜单中选择"复制Markdown链接"。然后您可以将此链接粘贴到另一个笔记的任何位置。

### 数学表示法

可以使用[KaTeX表示法](https://khan.github.io/KaTeX/)添加数学表达式。要添加内联方程式，请将表达式包裹在`$EXPRESSION$`中，例如`$\sqrt{3x-1}+(1+x)^2$`。要创建表达式块，请按如下方式包裹：

	$$
	EXPRESSION
	$$

例如：

	$$
	f(x) = \int_{-\infty}^\infty
		\hat f(\xi)\,e^{2 \pi i \xi x}
		\,d\xi
	$$

以下是Markdown和渲染结果并排的示例：

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/Katex.png" height="345px">

### 化学方程式

Joplin通过KaTeX的mhchem插件支持化学方程式。如果您启用数学表示法，此插件会自动启用。有关语法，请参阅[mhchem文档](https://mhchem.github.io/MathJax-mhchem/)。

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/Katex_mhchem.png" height="196px">

### 图表

您可以使用[Mermaid语法](https://mermaidjs.github.io/)在Joplin中创建图表。要添加这样的图表，请将Mermaid脚本包裹在"\`\`\`mermaid"代码块中，如下所示：

\`\`\`mermaid<br/>
graph TD;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;A-->B;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;A-->C;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;B-->D;<br/>
&nbsp;&nbsp;&nbsp;&nbsp;C-->D;<br/>
\`\`\`

以下是Markdown在左侧，渲染图表在右侧的效果：

![Joplin中的Mermaid支持](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/Mermaid.png)

请注意，无论当前主题如何，Mermaid图表总是在白色背景上渲染。这是因为它们可能包含各种颜色，这些颜色可能与当前主题不兼容。

### 复选框

可以按如下方式添加复选框：

	- [ ] 牛奶
	- [x] 大米
	- [ ] 鸡蛋

这将变成：

![Joplin中的复选框支持](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/Markdown_checkbox.jpg)

然后可以在移动和桌面应用程序中勾选这些复选框。

### HTML支持

通常建议以Markdown格式输入笔记，因为这使笔记更容易编辑。但是对于不支持某些功能的情况（如删除线或突出显示文本），您也可以直接使用HTML代码。例如，以下是有效的笔记：

	这是<s>删除线文本</s>与常规**Markdown**混合。

### Markdown插件

Joplin支持多个插件，可以切换打开/关闭以启用/禁用标准Markdown功能之外的markdown功能。这些插件列在下面。与常规插件不同，Markdown插件必须在[配置屏幕](https://github.com/laurent22/joplin/blob/dev/readme/apps/config_screen.md)的markdown部分中启用。并非所有插件默认都已启用，如果下面的"已启用"字段为"否"，那么请打开选项屏幕来启用该插件。可以以相同的方式禁用插件。

这些插件添加的功能不是CommonMark规范的一部分，因此，虽然它们在Joplin中都可以工作，但不能保证它们在其他Markdown阅读器中也能工作。通常这不是问题，但如果您需要与其他Markdown应用程序兼容，请记住这一点。

| 插件 | 语法 | 描述 | 已启用 | 截图 |
|--------|--------|-------------|---------|------------|
| 软换行 | 参见markdown-it演示中的[breaks](https://markdown-it.github.io/#md3=%7B%22source%22%3A%22This%20is%20line1%5CnThis%20is%20line2%5Cn%5CnThis%20is%20a%20line%20with%202%20trailing%20spaces%20%20%5CnNext%20line%5Cn%5CnClick%20the%20%60breaks%60%20checkbox%20above%20to%20see%20the%20difference.%5CnJoplin%27s%20default%20is%20hard%20breaks%20%28checked%20%60breaks%60%20checkbox%29.%22%2C%22defaults%22%3A%7B%22html%22%3Afalse%2C%22xhtmlOut%22%3Afalse%2C%22breaks%22%3Afalse%2C%22langPrefix%22%3A%22language-%22%2C%22linkify%22%3Afalse%2C%22typographer%22%3Afalse%2C%22_highlight%22%3Afalse%2C%22_strict%22%3Afalse%2C%22_view%22%3A%22html%22%7D%7D) | Joplin默认使用硬换行，这意味着换行符被渲染为`<br>`。启用软换行以获得传统markdown换行行为。 | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/softbreaks_plugin.jpg)
| 排版 | 参见markdown-it演示中的[typographer](https://markdown-it.github.io/#md3=%7B%22source%22%3A%22%23%20Typographic%20replacements%5Cn%5Cn%28c%29%20%28C%29%20%28r%29%20%28R%29%20%28tm%29%20%28TM%29%20%28p%29%20%28P%29%20%2B-%5Cn%5Cntest..%20test...%20test.....%20test%3F.....%20test!....%5Cn%5Cn!!!!!!%20%3F%3F%3F%3F%20%2C%2C%20%20--%20---%5Cn%5Cn%5C%22Smartypants%2C%20double%20quotes%5C%22%20and%20%27single%20quotes%27%5Cn%22%2C%22defaults%22%3A%7B%22html%22%3Afalse%2C%22xhtmlOut%22%3Afalse%2C%22breaks%22%3Afalse%2C%22langPrefix%22%3A%22language-%22%2C%22linkify%22%3Atrue%2C%22typographer%22%3Atrue%2C%22_highlight%22%3Atrue%2C%22_strict%22%3Afalse%2C%22_view%22%3A%22html%22%7D%7D) | 进行排版替换，(c) -&gt; © 等 | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/typographer_plugin.jpg) |
| 链接化 | 参见markdown-it演示中的[linkify](https://markdown-it.github.io/#md3=%7B%22source%22%3A%22Use%20the%20Linkify%20checkbox%20to%20switch%20link-detection%20on%20and%20off.%5Cn%5Cn%2A%2AThese%20links%20are%20auto-detected%3A%2A%2A%5Cn%5Cnhttps%3A%2F%2Fexample.com%5Cn%5Cnexample.com%5Cn%5Cntest%40example.com%5Cn%5Cn%2A%2AThese%20are%20always%20links%3A%2A%2A%5Cn%5Cn%5Blink%5D%28https%3A%2F%2Fjoplinapp.org%29%5Cn%5Cn%3Chttps%3A%2F%2Fexample.com%3E%22%2C%22defaults%22%3A%7B%22html%22%3Afalse%2C%22xhtmlOut%22%3Afalse%2C%22breaks%22%3Afalse%2C%22langPrefix%22%3A%22language-%22%2C%22linkify%22%3Atrue%2C%22typographer%22%3Atrue%2C%22_highlight%22%3Atrue%2C%22_strict%22%3Afalse%2C%22_view%22%3A%22html%22%7D%7D) | 自动检测URL并将其转换为可点击链接 | 是 |     |
| [Katex](https://katex.org) | `$$math expr$$`或`$math$` | [见上文](#数学表示法) | 是 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/katex_plugin.jpg) |
| [Fountain](https://fountain.io) | <code>\`\`\`fountain</code><br/>您的剧本...<br/><code>\`\`\`</code> | 添加对Fountain标记语言的支持，这是一种用于剧本创作的纯文本标记语言 | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/fountain_plugin.jpg) |
| [Mermaid](https://mermaid-js.github.io/mermaid/) | <code>\`\`\`mermaid</code><br/>mermaid语法...<br/><code>\`\`\`</code> | 有关完整描述，请参阅[插件页面](https://mermaid-js.github.io/mermaid/#/examples) | 是 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/mermaid.jpg) |
| [Mark](https://github.com/markdown-it/markdown-it-mark) | `==标记==` | 转换为`<mark>标记</mark>`（突出显示） | 是 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/mark_plugin.jpg) |
| [Footnote](https://github.com/markdown-it/markdown-it-footnote) | `简单的内联脚注 ^[我是内联的!]` | 有关完整描述，请参阅[插件页面](https://github.com/markdown-it/markdown-it-footnote) | 是 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/footnote_plugin.jpg) |
| [TOC](https://github.com/nagaozen/markdown-it-toc-done-right) | 任意一个`${toc}, [[toc]], [toc], [[_toc_]]` | 在目录页面的位置添加目录。基于标题和子标题 | 是 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/toc_plugin.jpg) |
| [Sub](https://github.com/markdown-it/markdown-it-sub) | `X~1~` | 转换为X<sub>1</sub> | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/sub_plugin.jpg) |
| [Sup](https://github.com/markdown-it/markdown-it-sup) | `X^2^` | 转换为X<sup>2</sup> | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/sup_plugin.jpg) |
| [Deflist](https://github.com/markdown-it/markdown-it-deflist) | 有关语法，请参阅[pandoc](http://johnmacfarlane.net/pandoc/README.html#definition-lists)页面 | 添加可通过markdown访问的html `<dl>`标签 | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/deflist_plugin.jpg) |
| [Abbr](https://github.com/markdown-it/markdown-it-abbr) | *[HTML]: 超文本标记语言<br/>HTML规范 | 允许定义缩写，稍后可以将鼠标悬停在上面以完整展开 | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/abbr_plugin.jpg) |
| [Emoji](https://github.com/markdown-it/markdown-it-emoji) | `:smile:` | 转换为😄。更多表情符号请参见[此列表](https://gist.github.com/rxaviers/7360908) | 否 |[查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/emoji_plugin.jpg) |
| [Insert](https://github.com/markdown-it/markdown-it-ins) | `++插入++` | 转换为`<ins>插入</ins>`（<ins>插入</ins>） | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/insert_plugin.jpg) |
| [Multitable](https://github.com/RedBug312/markdown-it-multimd-table) | 参见[MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html)页面 | 为markdown表格添加更多功能和自定义选项 | 否 | [查看](https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/md_plugins/multitable_plugin.jpg) | 