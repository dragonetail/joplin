# Joplin数据流程图

以下是基于代码分析得出的Joplin主要数据流程图，使用Mermaid图表格式描述。

## 核心数据流

```mermaid
flowchart TD
    User[用户] -->|输入| UI[用户界面]
    UI -->|提交操作| ActionDispatcher[Action分发器]
    ActionDispatcher -->|分发| Reducer[Reducer]
    Reducer -->|更新| State[应用状态]
    State -->|渲染| UI
    
    subgraph 数据读写
        Reducer <-->|读写| BaseModel[基础模型]
        BaseModel <-->|CRUD操作| JoplinDatabase[Joplin数据库]
        JoplinDatabase <-->|SQL操作| SQLite[(SQLite数据库)]
    end
    
    subgraph 同步流程
        User -->|触发同步| Synchronizer[同步器]
        Synchronizer <-->|读取变更| JoplinDatabase
        Synchronizer <-->|上传/下载| SyncTarget[同步目标]
        SyncTarget <-->|HTTP/API请求| RemoteStorage[(远程存储)]
    end
```

## 笔记编辑数据流

```mermaid
flowchart TD
    User[用户] -->|编辑| NoteEditor[笔记编辑器]
    NoteEditor -->|保存笔记| NoteModel[Note模型]
    NoteModel -->|保存| Database[(数据库)]
    
    NoteEditor -->|格式化显示| MarkdownRenderer[Markdown渲染器]
    MarkdownRenderer -->|获取资源| ResourceModel[Resource模型]
    ResourceModel <-->|读取资源| Database
    ResourceModel <-->|读取文件| FileSystem[(文件系统)]
    
    NoteEditor <-->|搜索替换| SearchEngine[搜索引擎]
    SearchEngine <-->|查询| Database
    
    NoteEditor -->|检查拼写| SpellChecker[拼写检查器]
```

## 搜索流程

```mermaid
flowchart TD
    User[用户] -->|搜索查询| SearchBar[搜索栏]
    SearchBar -->|请求| SearchEngine[搜索引擎]
    
    subgraph 搜索处理
        SearchEngine -->|全文检索| FTSIndex[全文搜索索引]
        SearchEngine -->|标题匹配| TitleSearch[标题搜索]
        SearchEngine -->|标签匹配| TagSearch[标签搜索]
        
        FTSIndex -->|查询| NormalizedNotes[(notes_normalized表)]
        TitleSearch -->|查询| Notes[(notes表)]
        TagSearch -->|查询| Tags[(tags表)]
    end
    
    SearchEngine -->|过滤结果| FilteredResults[过滤结果]
    FilteredResults -->|显示| SearchResultsList[搜索结果列表]
    SearchResultsList -->|选择结果| NoteEditor[笔记编辑器]
```

## 同步流程详细图

```mermaid
sequenceDiagram
    participant User as 用户
    participant UI as 用户界面
    participant Sync as 同步器
    participant DB as 本地数据库
    participant ST as 同步目标
    participant Remote as 远程存储
    
    User->>UI: 触发同步
    UI->>Sync: 开始同步
    
    Sync->>DB: 获取自上次同步后的变更
    DB-->>Sync: 返回本地变更
    
    Sync->>ST: 连接远程存储
    ST->>Remote: 认证请求
    Remote-->>ST: 认证成功
    
    Sync->>ST: 获取远程变更
    ST->>Remote: 请求变更
    Remote-->>ST: 返回远程变更
    ST-->>Sync: 返回远程变更列表
    
    Sync->>Sync: 解决冲突
    
    Sync->>ST: 上传本地变更
    ST->>Remote: 发送数据
    Remote-->>ST: 确认接收
    
    Sync->>DB: 应用远程变更
    DB-->>Sync: 确认更新
    
    Sync->>DB: 更新同步状态
    Sync-->>UI: 同步完成
    UI-->>User: 显示同步结果
```

这些图表基于对源代码的分析，展示了Joplin的主要数据流程。可以将这些Mermaid代码复制到支持Mermaid的Markdown查看器中渲染实际图表，或者使用在线Mermaid编辑器 (如https://mermaid.live) 进行可视化。 