# Joplin用户交互流程图

以下是基于代码分析得出的Joplin主要用户交互流程图，使用Mermaid图表格式描述。

## 主要用户界面交互流程

```mermaid
flowchart TD
    Start(开始) --> LaunchApp[启动应用]
    LaunchApp --> MainWindow[显示主窗口]
    
    MainWindow -->|侧边栏操作| FolderOps[笔记本操作]
    FolderOps -->|创建笔记本| CreateFolder[创建笔记本]
    FolderOps -->|重命名| RenameFolder[重命名笔记本]
    FolderOps -->|删除| DeleteFolder[删除笔记本]
    
    MainWindow -->|笔记列表操作| NoteOps[笔记操作]
    NoteOps -->|创建笔记| CreateNote[创建笔记]
    NoteOps -->|创建待办事项| CreateTodo[创建待办事项]
    NoteOps -->|排序| SortNotes[排序笔记]
    NoteOps -->|搜索| SearchNotes[搜索笔记]
    NoteOps -->|删除| DeleteNote[删除笔记]
    
    MainWindow -->|编辑器操作| EditorOps[编辑器操作]
    EditorOps -->|编辑内容| EditContent[编辑内容]
    EditorOps -->|格式化| FormatText[格式化文本]
    EditorOps -->|添加附件| AddAttachments[添加附件]
    EditorOps -->|切换视图| ToggleView[切换视图]
    
    MainWindow -->|主菜单操作| MenuOps[菜单操作]
    MenuOps -->|同步| SyncData[同步数据]
    MenuOps -->|导出| ExportData[导出数据]
    MenuOps -->|导入| ImportData[导入数据]
    MenuOps -->|设置| OpenSettings[打开设置]
    
    OpenSettings --> ConfigScreen[配置屏幕]
    ConfigScreen -->|保存设置| SaveSettings[保存设置]
    SaveSettings --> MainWindow
    
    SyncData -->|成功| ShowSuccess[显示成功]
    SyncData -->|失败| ShowError[显示错误]
    ShowSuccess --> MainWindow
    ShowError --> MainWindow
```

## macOS交互流程

```mermaid
flowchart TD
    Start(开始) --> LaunchApp[启动macOS应用]
    LaunchApp --> MainWindow[显示主窗口]
    
    MainWindow -->|菜单栏| MacMenuBar[macOS菜单栏]
    
    MacMenuBar -->|Joplin菜单| JoplinMenu[Joplin菜单]
    JoplinMenu -->|关于| AboutDialog[关于对话框]
    JoplinMenu -->|偏好设置| Preferences[偏好设置]
    JoplinMenu -->|隐藏/显示| ToggleVisibility[隐藏/显示]
    JoplinMenu -->|退出| QuitApp[退出应用]
    
    MacMenuBar -->|文件菜单| FileMenu[文件菜单]
    FileMenu -->|新建笔记| NewNote[新建笔记]
    FileMenu -->|新建待办事项| NewTodo[新建待办事项]
    FileMenu -->|新建笔记本| NewNotebook[新建笔记本]
    FileMenu -->|关闭窗口| CloseWindow[关闭窗口]
    FileMenu -->|打印| PrintNote[打印笔记]
    
    MacMenuBar -->|编辑菜单| EditMenu[编辑菜单]
    EditMenu -->|撤销/重做| UndoRedo[撤销/重做]
    EditMenu -->|剪切/复制/粘贴| CutCopyPaste[剪切/复制/粘贴]
    EditMenu -->|查找| Find[查找]
    
    MacMenuBar -->|视图菜单| ViewMenu[视图菜单]
    ViewMenu -->|切换侧边栏| ToggleSidebar[切换侧边栏]
    ViewMenu -->|切换笔记列表| ToggleNoteList[切换笔记列表]
    ViewMenu -->|切换布局| ToggleLayout[切换布局]
    
    MacMenuBar -->|工具菜单| ToolsMenu[工具菜单]
    ToolsMenu -->|同步| Sync[同步]
    ToolsMenu -->|加密| Encryption[加密]
    
    MacMenuBar -->|窗口菜单| WindowMenu[窗口菜单]
    WindowMenu -->|最小化| Minimize[最小化]
    WindowMenu -->|缩放| Zoom[缩放]
    
    MacMenuBar -->|帮助菜单| HelpMenu[帮助菜单]
    HelpMenu -->|文档| OpenDocs[打开文档]
    HelpMenu -->|论坛| OpenForum[打开论坛]
    HelpMenu -->|检查更新| CheckUpdates[检查更新]
```

## 触控手势交互 (macOS)

```mermaid
flowchart TD
    Start(开始) --> MacOSApp[macOS应用]
    
    MacOSApp -->|触控板手势| TouchpadGestures[触控板手势]
    
    TouchpadGestures -->|两指滚动| Scroll[滚动内容]
    TouchpadGestures -->|两指捏合| PinchToZoom[缩放内容]
    TouchpadGestures -->|两指轻点| RightClick[右键点击]
    TouchpadGestures -->|三指轻扫| SwipeNav[切换界面]
    
    MacOSApp -->|窗口操作| WindowGestures[窗口操作]
    WindowGestures -->|全屏手势| FullScreen[进入/退出全屏]
    WindowGestures -->|拖动窗口| MoveWindow[移动窗口]
    WindowGestures -->|调整大小| ResizeWindow[调整窗口大小]
    
    MacOSApp -->|编辑操作| EditGestures[编辑操作]
    EditGestures -->|按住Option| AlternativeActions[替代操作]
    EditGestures -->|按住Command| MultipleSelect[多选]
```

## 键盘快捷键流程

```mermaid
flowchart TD
    Start(开始) --> KeyboardInput[键盘输入]
    
    KeyboardInput -->|应用快捷键| AppShortcuts[应用快捷键]
    KeyboardInput -->|编辑器快捷键| EditorShortcuts[编辑器快捷键]
    KeyboardInput -->|导航快捷键| NavigationShortcuts[导航快捷键]
    
    AppShortcuts -->|Cmd+N| CreateNote[创建笔记]
    AppShortcuts -->|Cmd+T| CreateTodo[创建待办事项]
    AppShortcuts -->|Cmd+S| SaveNote[保存笔记]
    AppShortcuts -->|Cmd+P| PrintNote[打印笔记]
    AppShortcuts -->|Cmd+,| OpenPreferences[打开设置]
    AppShortcuts -->|Cmd+Q| QuitApp[退出应用]
    
    EditorShortcuts -->|Cmd+B| BoldText[加粗文本]
    EditorShortcuts -->|Cmd+I| ItalicText[斜体文本]
    EditorShortcuts -->|Cmd+K| CreateLink[创建链接]
    EditorShortcuts -->|Cmd+Z| UndoChange[撤销变更]
    EditorShortcuts -->|Cmd+Shift+Z| RedoChange[重做变更]
    
    NavigationShortcuts -->|Cmd+F| Search[搜索]
    NavigationShortcuts -->|Cmd+G| FindNext[查找下一个]
    NavigationShortcuts -->|Cmd+Shift+G| FindPrevious[查找上一个]
    NavigationShortcuts -->|Cmd+1/2/3| SwitchView[切换视图]
```

这些图表基于对源代码的分析，展示了Joplin的主要用户交互流程，特别是macOS平台的特定交互。可以将这些Mermaid代码复制到支持Mermaid的Markdown查看器中渲染实际图表，或者使用在线Mermaid编辑器 (如https://mermaid.live) 进行可视化。 