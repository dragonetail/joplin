# Joplin组件关系图

以下是基于代码分析得出的Joplin主要组件关系图，使用Mermaid图表格式描述。

## 核心组件关系

```mermaid
graph TD
    subgraph "前端应用"
        AppDesktop["app-desktop\n(桌面应用)"]
        AppMobile["app-mobile\n(移动应用)"]
        Editor["editor\n(编辑器)"]
        Renderer["renderer\n(Markdown渲染器)"]
    end
    
    subgraph "核心库"
        LibCore["lib\n(核心库)"]
        LibModels["models\n(数据模型)"]
        LibServices["services\n(服务层)"]
        LibSync["synchronizer\n(同步系统)"]
        LibPlugins["plugins\n(插件系统)"]
    end
    
    subgraph "数据层"
        Database["JoplinDatabase\n(数据库抽象)"]
        SQLStorage["SQLite存储"]
    end
    
    subgraph "同步服务"
        SyncTarget["SyncTarget\n(同步目标抽象)"]
        JoplinCloud["Joplin Cloud"]
        Dropbox["Dropbox"]
        OneDrive["OneDrive"]
        FileSystem["文件系统"]
        WebDAV["WebDAV"]
    end
    
    %% 应用与核心库的关系
    AppDesktop --> LibCore
    AppMobile --> LibCore
    AppDesktop --> Editor
    AppMobile --> Editor
    Editor --> Renderer
    
    %% 核心库内部关系
    LibCore --> LibModels
    LibCore --> LibServices
    LibCore --> LibSync
    LibCore --> LibPlugins
    
    %% 数据层关系
    LibModels --> Database
    Database --> SQLStorage
    
    %% 同步系统关系
    LibSync --> SyncTarget
    SyncTarget --> JoplinCloud
    SyncTarget --> Dropbox
    SyncTarget --> OneDrive
    SyncTarget --> FileSystem
    SyncTarget --> WebDAV
```

## 桌面应用组件关系

```mermaid
graph TD
    subgraph "桌面应用界面层"
        MainScreen["MainScreen\n(主屏幕)"]
        ConfigScreen["ConfigScreen\n(配置屏幕)"]
        NoteEditor["NoteEditor\n(笔记编辑器)"]
        SideBar["SideBar\n(侧边栏)"]
        NoteList["NoteList\n(笔记列表)"]
        MenuBar["MenuBar\n(菜单栏)"]
    end
    
    subgraph "响应式布局"
        ResizableLayout["ResizableLayout\n(可调整大小布局)"]
    end
    
    subgraph "主题系统"
        ThemeProvider["ThemeProvider\n(主题提供者)"]
        ThemeStyle["ThemeStyle\n(主题样式)"]
    end
    
    %% 界面组件关系
    MainScreen --> SideBar
    MainScreen --> NoteList
    MainScreen --> NoteEditor
    MainScreen --> MenuBar
    MainScreen --> ResizableLayout
    
    %% 主题关系
    ThemeProvider --> ThemeStyle
    ThemeStyle --> MainScreen
    ThemeStyle --> ConfigScreen
    ThemeStyle --> NoteEditor
```

## 插件系统

```mermaid
graph TD
    subgraph "插件系统"
        PluginService["PluginService\n(插件服务)"]
        PluginRunner["PluginRunner\n(插件运行器)"]
        PluginApi["PluginApi\n(插件API)"]
    end
    
    subgraph "扩展点"
        ContentScripts["ContentScripts\n(内容脚本)"]
        CommandRegistry["CommandRegistry\n(命令注册表)"]
        ViewRegistry["ViewRegistry\n(视图注册表)"]
        SettingsRegistry["SettingsRegistry\n(设置注册表)"]
    end
    
    %% 插件系统内部关系
    PluginService --> PluginRunner
    PluginRunner --> PluginApi
    
    %% 扩展点关系
    PluginApi --> ContentScripts
    PluginApi --> CommandRegistry
    PluginApi --> ViewRegistry
    PluginApi --> SettingsRegistry
```

这些图表基于对源代码的分析，展示了Joplin的主要组件及其之间的关系。可以将这些Mermaid代码复制到支持Mermaid的Markdown查看器中渲染实际图表，或者使用在线Mermaid编辑器 (如https://mermaid.live) 进行可视化。 