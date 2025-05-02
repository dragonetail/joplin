# Joplin应用打包与发布分析

本文档详细分析了Joplin应用的打包和发布流程，包括版本管理、构建过程、多平台打包配置以及各发布渠道的处理方式。

## 版本管理机制

### 版本号规范

Joplin采用标准的语义化版本(Semantic Versioning)规范，版本号格式为`主版本号.次版本号.补丁号`：

- **主版本号**：做了不兼容的API修改时增加
- **次版本号**：添加向下兼容的新功能时增加
- **补丁号**：做了向下兼容的问题修复时增加

### 版本发布周期

Joplin遵循定期发布模式，每年发布三个主要版本，遵循三阶段流程：

1. **发布阶段(Release)** - 周期开始，专注开发新特性
2. **冻结阶段(Freeze)** - 计划发布日期前两周，停止添加新功能，专注于稳定和修复bug
3. **发布阶段(Publishing)** - 正式发布新版本

### 版本号更新工具

Joplin使用专门的脚本来管理版本更新：

```bash
# 设置新的主版本.次版本号
yarn setupNewRelease 1.8
```

该脚本(`packages/tools/setupNewRelease.ts`)会执行以下操作：
- 更新所有包的版本号为指定的主次版本
- 更新包之间的依赖关系，确保内部依赖引用正确的版本

补丁号则在每个单独包的发布过程中自动递增，这通过各种发布脚本(`releaseDesktop`、`releaseAndroid`等)完成。

## 构建系统架构

### Monorepo结构

Joplin采用monorepo结构组织代码，使用Yarn workspaces管理多个子包：

```
packages/
  app-cli/          # 命令行应用
  app-desktop/      # 桌面应用
  app-mobile/       # 移动应用
  lib/              # 核心库
  renderer/         # Markdown渲染器
  tools/            # 构建工具
  ...               # 其他包
```

这种结构允许跨应用共享代码，同时简化版本和依赖管理。

### 构建工具链

Joplin使用多种构建工具：

- **Yarn**: 包管理和workspaces管理
- **TypeScript**: 代码编译
- **Webpack**: 资源打包和压缩
- **Gulp**: 任务自动化
- **electron-builder**: 桌面应用打包

## 桌面应用打包流程

桌面应用(Electron)的打包过程配置在`packages/app-desktop/package.json`文件中：

### 打包命令

```bash
# 执行打包
yarn dist

# 内部调用流程
yarn electronRebuild && npx electron-builder
```

### 关键配置

桌面应用打包配置在`package.json`的`build`字段中定义：

```json
"build": {
  "appId": "net.cozic.joplin-desktop",
  "productName": "Joplin",
  "afterSign": "./tools/notarizeMacApp.js",
  "extraResources": [
    "build/icons/**",
    "build/images/**",
    "build/defaultPlugins/**",
    "build/pdf.worker.min.js",
    "build/tesseract.js/**",
    "build/7zip/**"
  ],
  "asar": true,
  // 平台特定配置...
}
```

### 平台特定打包

#### Windows平台

```json
"win": {
  "icon": "../../Assets/ImageSources/Joplin.ico",
  "target": [
    { "target": "nsis", "arch": ["x64", "ia32"] },
    { "target": "portable", "arch": ["x64", "ia32"] }
  ],
  "extraFiles": [
    { "from": "build-win/Joplin.VisualElementsManifest.xml", "to": "." }
  ]
}
```

主要生成两种格式：
- NSIS安装程序(.exe)
- 便携版本(.exe)

#### macOS平台

```json
"mac": {
  "icon": "../../Assets/macOs.icns",
  "target": [
    { "target": "dmg", "arch": ["x64"] },
    { "target": "zip", "arch": ["x64"] }
  ],
  "hardenedRuntime": true,
  "notarize": false,
  "entitlements": "./build-mac/entitlements.mac.inherit.plist"
}
```

主要生成两种格式：
- DMG磁盘镜像(.dmg)
- ZIP压缩包(.zip)

macOS版本还包括公证(Notarization)过程，确保应用符合Apple的安全要求。

#### Linux平台

```json
"linux": {
  "icon": "../../Assets/LinuxIcons",
  "category": "Office",
  "target": ["AppImage", "deb"],
  "executableName": "joplin"
}
```

主要生成两种格式：
- AppImage自包含应用(.AppImage)
- Debian安装包(.deb)

### 持续集成构建

桌面应用通过GitHub Actions自动构建，流程由`.github/scripts/run_ci.sh`脚本控制：

```bash
if [ "$IS_DESKTOP_RELEASE" == "1" ]; then
  cd "$ROOT_DIR/packages/app-desktop"
  USE_HARD_LINKS=false yarn dist
fi
```

当推送版本标签到GitHub时，CI系统会自动构建应用并发布到GitHub Releases。

## 移动应用打包流程

### Android应用打包

Android应用通过以下命令打包和上传：

```bash
yarn releaseAndroid --type=prerelease
```

类型参数可以是`release`或`prerelease`，影响发布的版本类型。

Android应用使用React Native构建，通过Gradle生成APK和AAB文件。

### iOS应用打包

iOS应用需要使用XCode手动构建和发布，流程包括：
- 使用XCode打开iOS项目
- 配置签名证书和描述文件
- 构建应用并提交到App Store Connect

## CLI应用发布流程

CLI应用的发布流程与桌面和移动应用不同，它不打包依赖，而是从源代码安装：

1. 首先发布所有Joplin库：

```bash
yarn publishAll
```

2. 确保`app-cli/package.json`中的依赖引用正确的版本号：

```json
"dependencies": {
  "@joplin/lib": "1.8",
  "@joplin/renderer": "1.8"
}
```

3. 发布CLI应用：

```bash
yarn releaseCli
```

## 其他组件发布流程

### Web Clipper扩展

Web Clipper通过以下命令发布：

```bash
yarn releaseClipper
```

此命令会构建浏览器扩展并发布到各浏览器商店。

### 插件生成器

插件生成器的发布包含两个步骤：

1. 更新类型定义：
```bash
./updateTypes.sh
```

2. 发布生成器：
```bash
yarn releasePluginGenerator
```

### Joplin Server

服务器应用通过以下命令发布：

```bash
yarn releaseServer
```

## 插件打包系统

Joplin插件使用特定的打包系统，基于Webpack：

1. 插件使用Yeoman生成器创建：
```bash
yo joplin
```

2. 插件通过以下命令构建：
```bash
npm run dist
```

该命令执行三个操作：
- `buildMain`: 构建主要脚本
- `buildExtraScripts`: 构建额外脚本
- `createArchive`: 创建.jpl插件归档文件

3. 插件通过发布到npm实现分发：
```bash
npm publish
```

符合特定条件的npm包会自动被添加到Joplin插件库中：
- 包名以`joplin-plugin-`开头
- 关键字包含`joplin-plugin`
- `publish/`目录中有.jpl和.json文件

## 发布渠道

Joplin使用多种渠道发布应用：

### 正式发布

- **GitHub Releases**: 所有平台的正式版本
- **应用商店**: 
  - App Store (iOS)
  - Google Play (Android)
  - Microsoft Store (Windows)
- **Linux包管理器**: 通过第三方维护的包发布

### 预发布版本

Joplin也提供预发布版本，供用户测试新功能和报告问题：

- **GitHub预发布**: 标记为预发布的GitHub Releases
- **内部测试渠道**: 
  - TestFlight (iOS)
  - Google Play内部测试 (Android)

## 总结

Joplin的打包和发布系统是一个复杂而完善的流程，它包括：

1. **规范的版本管理**: 遵循语义化版本和固定发布周期
2. **多平台打包策略**: 为不同平台提供原生体验
3. **自动化构建流程**: 通过CI/CD实现持续集成和部署
4. **灵活的发布渠道**: 支持正式版和预发布版本

这套系统确保了Joplin应用在各平台上的一致性和稳定性，同时通过预发布流程收集用户反馈，持续改进产品质量。 