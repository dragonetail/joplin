# Joplin 终端应用程序

Joplin 是一款免费、开源的笔记和待办事项应用程序，可以处理大量组织到笔记本中的笔记。这些笔记可搜索、可复制、可标记和使用您自己的文本编辑器修改。

从 Evernote 通过 .enex 文件导出的笔记可以导入到 Joplin 中，包括格式化内容（转换为 Markdown）、资源（图像、附件等）和完整的元数据（地理位置、更新时间、创建时间等）。也可以导入纯 Markdown 文件。

笔记可以与各种目标[同步](#synchronisation)，包括文件系统（例如网络目录）、Nextcloud、Dropbox、OneDrive 或 WebDAV。同步笔记时，笔记本、标签和其他元数据被保存为纯文本文件，可以轻松检查、备份和移动。

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/ScreenshotTerminal.png" style="max-width: 60%">

## 安装

操作系统 | 方法
-----------------|----------------
macOS、Linux 或 Windows（通过 [WSL](https://docs.microsoft.com/en-us/windows/wsl/faq)） | 1. 首先，[安装 Node 12+](https://nodejs.org/en/download/package-manager/)。<br/><br/>2. 执行以下命令安装 Joplin 终端版：<br/>`NPM_CONFIG_PREFIX=~/.joplin-bin npm install -g joplin`<br/>`sudo ln -s ~/.joplin-bin/bin/joplin /usr/local/bin/joplin`<br><br>3. 输入以下命令启动 Joplin 终端版：<br>`joplin`<br><br>默认情况下，应用程序二进制文件将安装在 `~/.joplin-bin` 下。如果需要，您可以更改此目录。或者，如果您的 npm 权限设置如[此处](https://docs.npmjs.com/getting-started/fixing-npm-permissions#option-2-change-npms-default-directory-to-another-directory)（选项 2）所述，那么简单地运行 `npm -g install joplin` 就可以了。

### 不受支持的方法

还有其他安装终端应用程序的方法。但是，它们不受支持，问题必须报告给上游项目。

操作系统 | 方法
-----------------|----------------
Arch Linux       | Arch Linux 包可在[此处](https://aur.archlinux.org/packages/joplin/)获取。要安装它，请使用 AUR 包装器，如 yay：`yay -S joplin`。CLI 工具（输入 `joplin`）和桌面应用（输入 `joplin-desktop`）都已打包。您也可以使用 [chaotic-aur](https://wiki.archlinux.org/index.php/Unofficial_user_repositories#chaotic-aur) 存储库安装编译版本。如需支持，请前往 [GitHub 仓库](https://github.com/masterkorp/joplin-pkgbuild)。

## 使用方法

要启动应用程序，请输入 `joplin`。这将打开用户界面，它有三个主要窗格：笔记本、笔记和当前笔记的文本。还有可以通过[快捷键](#shortcuts)切换开关的其他面板。

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/ScreenshotTerminalCaptions.png" height="450px">

### 输入模式

Joplin 用户界面部分基于文本编辑器 Vim，并提供两种不同的模式来与笔记和笔记本交互：

#### 普通模式

允许使用 `Tab` 和 `Shift-Tab` 键在窗格之间移动，并使用箭头键选择/查看笔记。文本区域也可以使用箭头键滚动。按 `Enter` 编辑笔记。还有其他各种[快捷键](#shortcuts)可用。

#### 命令行模式

按 `:` 进入命令行模式。从那里，可以使用 Joplin 命令，如 `mknote` 或 `search`。请参阅[完整命令列表](#commands)。

可以通过标题或 ID 引用笔记或笔记本。但最简单的方法是使用以下快捷方式之一引用当前选定的项目：

快捷方式 | 描述
---------|------------
`$n`     | 引用当前选定的笔记
`$b`     | 引用当前选定的笔记本
`$c`     | 引用当前选定的项目。例如，如果笔记列表当前活动，`$c` 将引用当前选定的笔记。

**示例：**

创建标题为 "Wednesday's meeting" 的新笔记：

	mknote "Wednesday's meeting"

创建新的待办事项：

	mktodo "Buy bread"

将当前选定的笔记（$n）移动到标题为 "Personal" 的笔记本：

	mv $n "Personal"

将当前选定的笔记本（$b）重命名为 "Something"：

	ren $b "Something"

将本地文件附加到当前选定的笔记（$n）：

	attach $n /home/laurent/pictures/Vacation12.jpg

也可以从命令行模式更改配置。例如，要将当前编辑器更改为 Sublime Text：

	config editor "subl -w"

### 编辑笔记

要编辑笔记，请选择它并按 `ENTER`。或者，在命令行模式下，输入 `edit $n` 编辑当前选定的笔记，或输入 `edit "笔记标题"` 编辑特定笔记。

### 获取帮助

从命令行模式可以获取完整的使用信息，输入以下命令之一：

命令 | 描述
--------|-------------------
`help`  | 一般帮助信息
`help keymap` | 列出可用的快捷键
`help [命令]` | 显示有关特定命令的信息

如果帮助信息没有完全显示，请多次按 `Tab` 直到控制台获得焦点，然后使用箭头键或上下翻页滚动文本。

有关所有应用程序相关的一般信息，另请参阅 [Joplin 主页](https://joplinapp.org)。

## 快捷键

有两种类型的快捷键：直接操作用户界面的快捷键，如 `TAB` 用于在窗格之间移动；以及简单的命令快捷方式。以类似于 Vim 的方式，这些快捷方式通常是动词后跟对象。例如，输入 `mn`（[m]ake [n]ote 的缩写）用于创建新笔记：它会将界面切换到命令行模式并预填充 `mknote ""`，从这里可以输入笔记的标题。下面是默认快捷键的完整列表：

	:                 进入命令行模式
	TAB               聚焦下一个
	SHIFT_TAB         聚焦上一个
	UP                上移
	DOWN              下移
	PAGE_UP           向上翻页
	PAGE_DOWN         向下翻页
	ENTER             激活
	DELETE, BACKSPACE 删除
	(SPACE)           切换待办事项状态 $n
	n                 下一个链接
	b                 上一个链接
	o                 打开链接
	tc                切换控制台
	tm                切换元数据
	/                 搜索 ""
	mn                创建笔记 ""
	mt                创建待办事项 ""
	mb                创建笔记本 ""
	yn                复制笔记 $n ""
	dn                移动笔记 $n ""

可以通过在配置目录中添加一个 keymap 文件来配置快捷键：`~/.config/joplin/keymap.json`。此文件的内容是一个 JSON 数组，每个条目定义一个命令及其关联的键。

作为示例，这是默认的键映射，但请阅读下面对每个属性的详细解释。

```json
[
	{ "keys": [":"], "type": "function", "command": "enter_command_line_mode" },
	{ "keys": ["TAB"], "type": "function", "command": "focus_next" },
	{ "keys": ["SHIFT_TAB"], "type": "function", "command": "focus_previous" },
	{ "keys": ["UP"], "type": "function", "command": "move_up" },
	{ "keys": ["DOWN"], "type": "function", "command": "move_down" },
	{ "keys": ["PAGE_UP"], "type": "function", "command": "page_up" },
	{ "keys": ["PAGE_DOWN"], "type": "function", "command": "page_down" },
	{ "keys": ["ENTER"], "type": "function", "command": "activate" },
	{ "keys": ["DELETE", "BACKSPACE"], "type": "function", "command": "delete" },
	{ "keys": [" "], "command": "todo toggle $n" },
	{ "keys": ["n"], "type": "function", "command": "next_link" },
	{ "keys": ["b"], "type": "function", "command": "previous_link" },
	{ "keys": ["o"], "type": "function", "command": "open_link" },
	{ "keys": ["tc"], "type": "function", "command": "toggle_console" },
	{ "keys": ["tm"], "type": "function", "command": "toggle_metadata" },
	{ "keys": ["/"], "type": "prompt", "command": "search \"\"", "cursorPosition": -2 },
	{ "keys": ["mn"], "type": "prompt", "command": "mknote \"\"", "cursorPosition": -2 },
	{ "keys": ["mt"], "type": "prompt", "command": "mktodo \"\"", "cursorPosition": -2 },
	{ "keys": ["mb"], "type": "prompt", "command": "mkbook \"\"", "cursorPosition": -2 },
	{ "keys": ["yn"], "type": "prompt", "command": "cp $n \"\"", "cursorPosition": -2 },
	{ "keys": ["dn"], "type": "prompt", "command": "mv $n \"\"", "cursorPosition": -2 }
]
```

每个条目可以有以下属性：

名称 | 描述
-----|------------
`keys` | 触发操作的键数组。特殊键，如上翻页、向下箭头等，需要大写指定。查看[可用特殊键列表](https://github.com/cronvel/terminal-kit/blob/3114206a9556f518cc63abbcb3d188fe1995100d/lib/termconfig/xterm.js#L531)。例如，`['DELETE', 'BACKSPACE']` 表示用户按下删除键或退格键时命令将运行。也可以提供键组合 - 在这种情况下，请小写指定。例如，"tc" 表示用户按下 "t" 然后按 "c" 时将执行命令。特殊键也可以以这种方式使用 - 只需一个接一个地写出即可。例如，`CTRL_WCTRL_W` 表示如果用户按下 "ctrl-w ctrl-w"，则会执行该操作。
`type` | 命令类型。可以是 "exec"、"function" 或 "prompt"。**exec**：简单执行提供的[命令](#commands)。例如 `edit $n` 将编辑选定的笔记。**function**：运行特殊命令（参见下面的函数列表）。**prompt**：与 "exec" 有点类似，除了命令不会立即执行 - 这允许用户提供附加数据。例如 `mknote ""` 会用此命令填充命令行并允许用户设置标题。提示命令还可以接受 `cursorPosition` 参数（见下文）
`command` | 需要执行的命令
`cursorPosition` | 一个整数。对于提示命令，告诉光标（插入符号）应该从哪里开始。这方便定位光标，例如在引号之间。使用负值从末尾设置位置。值为 "0" 表示将光标定位在第一个字符处。值为 "-1" 表示将其定位在末尾。

这是特殊函数列表：

名称 | 描述
-----|------------
enter_command_line_mode | 进入命令行模式
focus_next | 聚焦下一个窗格（或小部件）
focus_previous | 聚焦上一个窗格（或小部件）
move_up | 向上移动（例如在列表中）
move_down | 向下移动（例如在列表中）
page_up | 向上翻页
page_down | 向下翻页
next_link | 选择当前打开笔记中的下一个链接（如果当前没有选择链接，将选择第一个链接）
previous_link | 选择当前打开笔记中的上一个链接（如果当前没有选择链接，将选择最后一个链接）
open_link | 在外部打开当前选定的链接
activate | 激活选定的项目。例如，如果项目是笔记，它将在编辑器中打开
delete | 删除选定的项目
toggle_console | 切换控制台
toggle_metadata | 切换笔记元数据

## 命令

在[命令行模式](#command-line-mode)下可以使用以下命令：

```plaintext
attach <note> <file>

	将给定文件附加到笔记。

batch <file-path>

	运行文本文件中包含的命令。每行应该有一个命令。

cat <note>

	显示给定的笔记。

	-v, --verbose  显示关于笔记的完整信息。

config [name] [value]

	获取或设置配置值。如果未提供 [value]，将显示 [name] 的值。
	如果 [name] 和 [value] 都未提供，将列出当前配置。

	-v, --verbose         还显示未设置和隐藏的配置变量。
	--export              将所有设置以 JSON 格式写入 STDOUT，包括安全变量。
	--import              从 STDIN 读取 JSON 格式的设置。
	--import-file <file>  从 <file> 读取设置。<file> 必须包含有效的 JSON。

可能的键/值：

	sync.target                    同步目标。
									要同步到的目标。每个同步目标可能有额外的参数，
									命名为 `sync.NUM.NAME`（下面都有文档）。
									类型：枚举。
									可能的值：0（(无)）、2（文件系统）、3（OneDrive）、
									5（Nextcloud）、6（WebDAV）、7（Dropbox）、
									8（S3（测试版））、9（Joplin 服务器（测试版））、
									10（Joplin Cloud）。
									默认值：0

	sync.2.path                    要同步的目录（绝对路径）。
									注意：如果您更改此位置，请确保在同步前将所有内容
									复制到其中，否则所有文件将被删除！
									有关更多详细信息，请参阅常见问题解答：
									https://joplinapp.org/faq/
									类型：字符串。

	sync.5.path                    Nextcloud WebDAV URL。
									注意：如果您更改此位置，请确保在同步前将所有内容
									复制到其中，否则所有文件将被删除！
									有关更多详细信息，请参阅常见问题解答：
									https://joplinapp.org/faq/
									类型：字符串。

	sync.5.username                Nextcloud 用户名。
									类型：字符串。

	sync.5.password                Nextcloud 密码。
									类型：字符串。

	sync.6.path                    WebDAV URL。
									注意：如果您更改此位置，请确保在同步前将所有内容
									复制到其中，否则所有文件将被删除！
									有关更多详细信息，请参阅常见问题解答：
									https://joplinapp.org/faq/
									类型：字符串。

	sync.6.username                WebDAV 用户名。
									类型：字符串。

	sync.6.password                WebDAV 密码。
									类型：字符串。

	sync.8.path                    AWS S3 存储桶。
									注意：如果您更改此位置，请确保在同步前将所有内容
									复制到其中，否则所有文件将被删除！
									有关更多详细信息，请参阅常见问题解答：
									https://joplinapp.org/faq/
									类型：字符串。

	sync.8.url                     AWS S3 URL。
									类型：字符串。
									默认值："https://s3.amazonaws.com/"

	sync.8.region                  AWS 区域。
									类型：字符串。

	sync.8.username                AWS 访问密钥。
									类型：字符串。

	sync.8.password                AWS 密钥。
									类型：字符串。

	sync.8.forcePathStyle          强制路径样式。
									类型：布尔值。
									默认值：false

	sync.9.path                    Joplin 服务器 URL。
									注意：如果您更改此位置，请确保在同步前将所有内容
									复制到其中，否则所有文件将被删除！
									有关更多详细信息，请参阅常见问题解答：
									https://joplinapp.org/faq/
									类型：字符串。

	sync.9.username                Joplin 服务器电子邮件。
									类型：字符串。

	sync.9.password                Joplin 服务器密码。
									类型：字符串。

	sync.10.username               Joplin Cloud 电子邮件。
									类型：字符串。

	sync.10.password               Joplin Cloud 密码。
									类型：字符串。

	sync.maxConcurrentConnections  最大并发连接数。
									如果同步期间出现"net::ERR_INSUFFICIENT_
									RESOURCES"错误，请降低此值。
									类型：整数。
									默认值：5

	editor                         要使用的编辑器。
									请注意，您也可以将参数传递给编辑器，
									例如"vim -b"或"\"c:\\program files\\notepad++\\
									notepad++.exe\" -multiInst -nosession"。
									类型：字符串。
									默认值：

	noteVisiblePanes               Markdown 编辑器中可见的窗格。
									通过在数组中包含相关值启用或禁用。例如，要启用
									编辑器和查看器窗格，请使用 ["editor", "viewer"]。
									类型：数组。
									可能的值：["editor", "viewer", "sidebar"]。
									默认值：["editor", "viewer"]

	ui.layout                      选择垂直或水平布局。
									类型：枚举。
									可能的值：1（使用垂直布局）、2（使用水平布局）。
									默认值：1

	trackLocation                  新建笔记时自动设置地理位置。
									类型：布尔值。
									默认值：true

	notes.sortOrder.field          排序笔记的字段。
									类型：枚举。
									可能的值：0（用户顺序）、1（标题）、2（按时间更新）、
									3（按时间创建）、4（按源网址）。
									默认值：2

	notes.sortOrder.reverse        对笔记排序顺序进行反转。
									类型：布尔值。
									默认值：false

	folders.sortOrder.field        排序文件夹的字段。
									类型：枚举。
									可能的值：0（用户顺序）、1（标题）、
									2（按上次笔记更新时间排序）、
									3（按创建时间排序）。
									默认值：1

	folders.sortOrder.reverse      对文件夹排序顺序进行反转。
									类型：布尔值。
									默认值：false

	languages                      语言环境。
									类型：字符串。
									默认值：

	sync.interval                  自动同步的间隔（以秒为单位）。
									不推荐使用！在桌面应用程序中使用。
									CLI 客户端在终端中运行时自动停止，
									所以使用 "cron" 来自动同步。
									类型：整数。

	timeFormat                     日期格式。
									类型：字符串。
									默认值：

	showAdvancedOptions            是否显示高级选项。例如，在配置命令中 
									显示安全选项。
									类型：布尔值。
									默认值：false

	sync.clearLocalSyncStateShownCount
	                              多少次向用户显示这个选项？
									由软件内部使用，手动设置可能会破坏同步功能！
									类型：整数。
									默认值：0

cp <note> [notebook]

	将所选笔记复制到指定的笔记本。

	-d, --duplicate      生成笔记的重复副本
	-f, --force-overlay  完全覆盖现有笔记（注意：这将删除添加到目标笔记本中的任何现有更改）

dn <notebook>

	以递归方式删除所选笔记本及其所有笔记和子笔记本。

	-f, --force  跳过确认步骤

e2ee

	端到端加密命令。

	disable                           完全禁用端到端加密

	enable                            完全启用端到端加密

	decrypt <password>                解密数据
		--retry-failed-items          重试之前失败的项目。
		--retry-only                  只重试之前失败的项目。
		--master-key-id <mkid>        要使用的主密钥的 ID。

	encrypt <password>                加密数据
		--retry-failed-items          重试之前失败的项目。
		--retry-only                  只重试之前失败的项目。
		--master-key-id <mkid>        要使用的主密钥的 ID。

	status                            显示当前 E2EE 状态

	target-status [target]            显示给定目标的 E2EE 状态

	import-master-key path            导入主密钥

	export-master-key [id] path       导出主密钥

edit <note>

	编辑选定的笔记。

	-n, --note  编辑笔记属性。

exit

	退出应用程序。

export <path> [format]

	将所选笔记本导出为指定格式的 <path>。

	format                导出格式。可以是"jex"（默认）、"raw"或"md"（Markdown）。
	--note <note>         导出时包括指定的笔记。可以多次使用该选项。
	--notebook <notebook> 导出时包括指定的笔记本。可以多次使用该选项。
	--all-notes           导出所有笔记。
	--even-encrypted-ones 即使已经加密，也导出笔记。

geoloc <note>

	设置或显示选定笔记的地理位置。

	-n, --note                   显示笔记的地理位置。

help [command]

	显示使用信息。

import <path> [notebook]

	将文件导入选定笔记本。

	--format <format>  源格式：可以是"jex"、"md"、"raw"或"enex"。要在不同类型中选择，
	                  请提供文件扩展名。

ls [pattern]

	显示笔记、笔记本或标签列表。

	-l, --long            显示关于笔记的额外信息。
	-n, --limit <num>     仅显示给定数量的笔记。
	-s, --sort <field>    按给定字段排序：updated_time（默认），
	                      created_time，title，user_updated_time，
	                      user_created_time。
	-r, --reverse         反转排序顺序。
	-t, --type <type>     指定要显示的项目类型：note（默认），folder或tag。
	-f, --format <format> 以指定格式显示结果：text（默认），csv 或 json。
	-a, --archived        显示已归档（已完成）的待办事项）。
	-d, --deleted         显示已删除的笔记（从回收站中）

mkbook <new-notebook>

	创建新笔记本。

mknote <new-note>

	在当前选定的笔记本中创建新笔记。

	-d, --date <date>     用此日期/时间设置创建日期（使用 ISO 格式，例如"2021-11
	                      -21T12:00:00.000+01:00"）

mktodo <new-todo>

	在当前选定的笔记本中创建新待办事项。

mv <note> [notebook]

	将所选笔记移动到指定的笔记本。

	-f, --force-overlay  完全覆盖现有笔记（注意：这将删除添加到目标笔记本中的任何现有更改）

render <note>

	将所选笔记呈现为指定格式的 stdout。

	--format <format>  输出格式。可以是"html"（默认）或"txt"。

ren <item> <name>

	重命名笔记或笔记本。

rmbook <notebook>

	删除选定的笔记本。笔记本中的所有笔记都将被移动到回收站。

	-f, --force  跳过确认步骤

search <pattern> [notebook]

	在选定的笔记本（或指定的笔记本）中搜索给定模式。

	-n, --limit <num>      仅展示给定数量的结果
	-a, --archived         还在已归档笔记中搜索
	-p, --pre-lines <num>  包含匹配行之前的行数（命令行）
	-A, --post-lines <num> 包含匹配行之后的行数（命令行）

server <command> [port]

	启动或停止内部服务器

	Commands:
	  start          启动服务器
	  stop           停止服务器
	  status         显示服务器状态

set <note> <name> [value]

	设置笔记属性。

	-v, --verbose  在设置属性之前显示属性

status

	同步状态信息。

	-l, --long  显示有关同步期间发生的更改的完整信息

sync

	同步笔记。

	--target <target>       设置同步目标。
	-f, --force             强制完全同步。
	--retry-failed-items    重试之前失败的项目。
	--use-lock              仅限测试用途 - 同步前获取锁。

tag <tag-command> [tag] [note]

	标签管理。

	<tag-command> 可以是:
	    add:    将一个或多个标签添加到笔记
	    remove: 从笔记中删除一个或多个标签
	    toggle: 切换一个或多个标签的状态（比如移除已添加的标签，添加已移除的标签）
	    list:   列出出所有笔记的所有标签
	    notetags: 列出指定笔记的所有标签

todo <todo-command> <note>

	待办事项管理。

	<todo-command> 可以是:
	    toggle: 将"待办"状态在"待完成"和"已完成"之间切换

	-c, --completed         将待办状态设置为"已完成"
	-a, --active            将待办状态设置为"待完成"

undone <command>

	撤销命令。

	<command> 可以是:
	    edit:    撤销上一次编辑操作

use <notebook>

	切换到特定笔记本。

version

	显示版本信息。

	--log                  将当前软件版本保存到日志中。
```

## URL链接

当按下Ctrl+点击URL（或在高亮显示时使用快捷键'o'打开）时，大多数终端将在默认浏览器中打开该URL。然而，一个问题，特别是对于长URL，它们可能会像这样显示：

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/UrlCut.png" width="300px">

这不仅使文本难以阅读，而且链接被切成两部分，也无法点击。

作为解决方案，Joplin尝试在后台启动一个迷你服务器，如果成功，所有链接都将转换为更短的URL：

<img src="https://raw.githubusercontent.com/laurent22/joplin/dev/Assets/WebsiteAssets/images/UrlNoCut.png" width="300px">

由于这仍然是一个实际的URL，终端仍然会使其可点击。而且由于URL较短，文本更易读，链接也不太可能被切断。资源（附加到笔记的文件）和外部链接都以这种方式处理。

## 附件/资源

在Markdown中，资源链接表示为资源的简单ID。为了让用户访问这些资源，它们将像链接一样被转换为本地URL。点击此链接将打开浏览器，浏览器将处理该文件——即显示图像、打开PDF文件等。

## Shell模式

命令也可以直接从shell中使用。要查看可用命令列表，请输入`joplin help all`。要引用笔记、笔记本或标签，您可以使用ID（输入`joplin ls -l`查看ID）或标题。

例如，这将在"My notebook"笔记本中创建一个新笔记"My note"：

	$ joplin mkbook "My notebook"
	$ joplin use "My notebook"
	$ joplin mknote "My note"

要查看新创建的笔记：

	$ joplin ls -l
	fe889 07/12/2017 17:57 My note

为笔记提供新标题：

	$ joplin set fe889 title "New title"

## 许可证

版权所有 (c) 2016-2023 Laurent Cozic

特此免费授予任何获得本软件及相关文档文件（"软件"）副本的人不受限制地处理本软件的权利，包括但不限于使用、复制、修改、合并、出版、分发、再许可和/或销售本软件的副本，以及允许本软件的接收者这样做，但须符合以下条件：

上述版权声明和本许可声明应包含在本软件的所有副本或实质性部分中。

本软件按"原样"提供，没有任何形式的明示或暗示的保证，包括但不限于对适销性、特定用途的适用性和非侵权性的保证。在任何情况下，作者或版权持有人均不对任何索赔、损害或其他责任负责，无论是在合同诉讼、侵权行为或其他方面，由软件或软件的使用或其他交易引起，与软件有关或与之有关。 