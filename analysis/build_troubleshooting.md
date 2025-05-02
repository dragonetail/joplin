# Joplin构建问题与解决方案

本文档记录了在构建Joplin项目过程中可能遇到的常见问题及其解决方案。

## macOS构建问题

### `canvas`包安装失败

在macOS上使用`yarn install`时，可能会遇到`canvas`包构建失败的问题，错误信息类似：

```
canvas@npm:2.11.2 couldn't be built successfully (exit code 1, logs can be found here: /private/var/folders/.../build.log)
```

#### 解决方案

可以使用以下命令跳过构建步骤，完成依赖安装：

```bash
export PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1 && yarn install --mode=skip-build
```

这个命令可以：
1. 跳过Playwright浏览器的下载（通过环境变量）
2. 使用Yarn的`skip-build`模式安装依赖，避免编译需要本地编译的包

### 其他macOS依赖问题

如果需要完整构建（包括canvas等原生依赖），确保安装了以下系统依赖：

```bash
brew install pkg-config cairo pango libpng jpeg giflib vips
```

## 跨平台构建说明

不同平台可能需要不同的系统依赖，请参考官方文档中的[构建疑难解答](https://github.com/laurent22/joplin/blob/dev/readme/dev/build_troubleshooting.md)获取更多信息。

## 推荐的构建环境

推荐使用项目中配置的Devbox环境进行构建，它可以自动处理所需的系统依赖：

```bash
# 安装Devbox（如果尚未安装）
curl -fsSL https://get.jetify.com/devbox | bash

# 使用Devbox shell
devbox shell

# 在Devbox环境中安装依赖
yarn install
``` 