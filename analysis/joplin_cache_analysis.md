# Joplin的本地缓存机制分析

经过对Joplin源代码的分析，我们可以总结出Joplin使用的主要本地缓存和数据存储机制。

## 主要存储机制

### 1. SQLite数据库

Joplin的核心数据存储使用SQLite数据库，这是应用程序最主要的持久化存储方式。

#### 数据库位置
```javascript
// packages/lib/BaseApplication.ts
await this.database_.open({ name: `${profileDir}/database.sqlite` });
```

数据库文件存储在用户的配置文件目录中，通常是：
- macOS: `~/Library/Application Support/joplin-desktop`
- Windows: `%APPDATA%\joplin-desktop`
- Linux: `~/.config/joplin-desktop`

#### 主要数据表
Joplin的SQLite数据库包含多个表，主要包括：

- `notes`: 存储笔记内容，包含`body`字段保存Markdown文本
- `folders`: 存储笔记本结构
- `tags`: 存储标签
- `resources`: 存储附件和图片的元数据
- `settings`: 存储应用设置
- `sync_items`: 同步相关信息
- `deleted_items`: 已删除的项目
- `note_tags`: 笔记和标签的关联关系
- `notes_normalized`: 用于全文搜索的标准化笔记内容
- `notes_fts`: 全文搜索索引表

### 2. 临时缓存 (node-persist)

Joplin使用`node-persist`库在临时目录创建轻量级的缓存存储。

#### 缓存位置和配置
```javascript
// packages/lib/Cache.js
Cache.storage = async function() {
  if (Cache.storage_) return Cache.storage_;
  Cache.storage_ = require('node-persist');
  await Cache.storage_.init({ dir: `${require('os').tmpdir()}/joplin-cache`, ttl: 1000 * 60 });
  return Cache.storage_;
};
```

这个缓存使用操作系统的临时目录(`os.tmpdir()`)，并设置了默认60秒的TTL(Time To Live)。

#### 缓存用途
这个临时缓存主要用于：
- 存储频繁访问但不需要长期保存的数据
- 提高应用性能，减少对主数据库的访问
- 存储计算结果和中间数据

### 3. 缓存目录

应用还维护了一个专门的缓存目录，用于存储其他类型的缓存数据：

```javascript
// packages/lib/BaseApplication.ts
const cacheDir = `${profileDir}/cache`;
Setting.setConstant('cacheDir', cacheDir);
await fs.mkdirp(cacheDir, 0o755);
```

#### 缓存目录用途
这个目录用于存储：
- 插件临时文件
- 导出临时文件
- 共享文件临时拷贝
- 语音识别模型缓存
- 外部工具临时文件(如7zip)
- 渲染缓存

### 4. 资源文件存储

除了数据库之外，Joplin还维护一个资源目录用于存储附件和图片的实际内容：

```javascript
// packages/lib/BaseApplication.ts
const resourceDir = `${profileDir}/${resourceDirName}`;
Setting.setConstant('resourceDir', resourceDir);
```

## 缓存机制的实现细节

### 编辑器和渲染缓存

Joplin为提高性能实现了多种缓存策略：

1. **Markdown渲染缓存**：
   ```typescript
   // packages/renderer/MdToHtml/rules/katex.ts
   const cache_: any = {};
   function renderToStringWithCache(latex: string, katexOptions: any) {
     const cacheKey = md5(escape(latex) + escape(stringifyKatexOptions(katexOptions)));
     if (cacheKey in cache_) {
       return cache_[cacheKey];
     }
     // ...渲染代码...
     cache_[cacheKey] = output;
     return output;
   }
   ```

2. **HTML净化缓存**：
   ```typescript
   // packages/renderer/MdToHtml/rules/sanitize_html.ts
   const cacheKey = md5(escape(token.content));
   let sanitizedContent = ruleOptions.context.cache.value(cacheKey);
   // ...
   ruleOptions.context.cache.setValue(cacheKey, sanitizedContent, 1000 * 60 * 60);
   ```

3. **主题样式缓存**：
   ```typescript
   // packages/app-mobile/components/global-style.ts
   const themeCache_: Record<string, ThemeStyle> = {};
   const cacheKey = [theme].join('-');
   if (themeCache_[cacheKey]) return themeCache_[cacheKey];
   // ...
   themeCache_[cacheKey] = output;
   ```

### 搜索引擎缓存

Joplin使用FTS (Full Text Search) 表来加速搜索：

```sql
-- 创建全文搜索表
CREATE VIRTUAL TABLE notes_fts USING fts4(content="notes_normalized", notindexed="id", id, title, body);
```

并通过触发器保持搜索索引与内容的同步：

```sql
-- 同步触发器
CREATE TRIGGER notes_after_update AFTER UPDATE ON notes_normalized BEGIN
  INSERT INTO notes_fts(docid, id, title, body) SELECT rowid, id, title, body FROM notes_normalized WHERE new.rowid = notes_normalized.rowid;
END;
```

## 缓存清理和维护

1. **临时缓存自动过期**：
   - node-persist缓存设置了60秒TTL
   - 超过TTL的条目会在下次访问时过期

2. **长期缓存手动清理**：
   - 提供了API用于删除特定类型的缓存，如语音识别模型缓存
   - 临时文件共享完成后会清理

3. **应用启动时清理**：
   ```typescript
   // packages/lib/BaseApplication.ts
   await shim.fsDriver().removeAllThatStartWith(profileDir, 'edit-');
   ```

## 结论

Joplin的本地缓存机制是多层次的：
1. 核心数据存储在SQLite数据库中
2. 附件和图片实际内容存储在资源目录
3. 临时数据使用node-persist缓存在操作系统临时目录
4. 其他缓存数据存储在专门的缓存目录

这种设计提供了良好的性能和数据持久性平衡，同时支持离线工作和数据同步。 