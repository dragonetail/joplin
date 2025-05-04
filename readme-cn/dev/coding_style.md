# 编码风格

编码风格主要由运行`eslint`的预提交钩子强制执行。此钩子在应用程序目录中的任何位置运行`yarn install`时安装。如果由于某种原因预提交钩子没有安装，您可以通过在仓库根目录运行`yarn install`手动安装它。

## 使用eslint强制执行规则

只要可能，应该使用eslint规则强制执行编码风格。为此，请在`eslintrc.js`中添加相关规则或插件。要手动运行linter，请从项目根目录运行`yarn linter ./`。

添加规则时，您经常会发现许多文件将不再通过linter检查。在这种情况下，您有两个选择：

- 一个一个地修复文件。如果文件不太多，并且更改简单（不太可能引入回归），这是首选的解决方案。

- 或者使用`yarn linter-interactive ./`禁用现有错误。交互式工具将处理所有文件，然后您可以选择禁用它找到的任何现有错误（通过在其上方添加`eslint-disable-next-line`注释）。这允许保持现有的、工作的代码库不变，并强制新代码遵循规则。使用此方法时，添加注释"Old code before rule was applied"（应用规则前的旧代码），这样我们可以轻松找回所有已自动禁用的行。

## TypeScript规则

### 创建新的`.ts`文件

因为TypeScript编译器会生成`.js`文件，请确保将这些新的`.js`文件添加到`.eslintignore`和`.gitignore`中。

为此，
1. 如果TypeScript编译器已经为新的`.ts`文件生成了`.js`文件，请删除它。
2. 在项目根目录运行`yarn updateIgnored`（或`yarn postinstall`）

### 在修改前将现有的`.js`文件转换为TypeScript

即使您正在**修改**最初是JavaScript的文件，理想情况下也应该先将其转换为TypeScript再进行修改。

但是，如果这是一个大文件，请先询问是否需要转换。由于类型定义不明确，一些非常旧和大的JS文件很难正确转换，因此在某些情况下最好将其留待他日（或另一个PR）。

### 优先使用`import`而不是`require`

在TypeScript文件中，优先使用`import`而不是`require`，这样我们可以受益于类型检查。如果不能正常工作，您可能需要通过`yarn add @types/NAME_OF_PACKAGE`添加类型。如果您尝试导入一个旧包，它可能没有TypeScript类型，在这种情况下使用`require()`是可以接受的。

### 避免内联类型

通常，请单独定义类型，因为这样可以提高可读性，并且意味着类型可以被重复使用。

**不好的做法：**
```ts
const config: { [key: string]: Knex.Config } = {
	// ...
}	
```

**好的做法：**
```ts
type Config = Record<string, Knex.Config>;

const config: Config = {
	// ...
}	
```

### 当类型可以被推断时不要设置类型

TypeScript可以自动检测类型，因此在许多情况下不需要显式设置类型，这会使代码不必要地冗长。我们已经启用了eslint规则`no-inferable-types`，但它只适用于字符串、数字等简单类型，不适用于函数调用。

**不好的做法：**
```ts
const getSomething():string => {
	return 'something';
}

const timestamp:number = Date.now();
```

**好的做法：**
```ts
const getSomething() => {
	return 'something';
}

const timestamp = Date.now();
```

## 文件名、导入和导出

### 文件名

 * `camelCase.ts`：导出多个内容的文件。
   * 示例：[`checkForUpdates.ts`](https://github.com/laurent22/joplin/blob/dev/packages/app-desktop/checkForUpdates.ts)
 * `PascalCase.ts`：[仅当文件包含一个是默认导出的单一类时。](https://github.com/laurent22/joplin/pull/6607#discussion_r906847156)
 * `types.ts`或`fooTypes.ts`：[共享类型定义](https://github.com/laurent22/joplin/pull/6607#discussion_r906847156)
   * 示例：[`types.ts`](https://github.com/laurent22/joplin/blob/dev/packages/server/src/utils/types.ts)

### 导入和导出成员使用相同的大小写

如果您创建一个导出名为`processData()`的单一函数的文件，则该文件应命名为`processData.ts`。导入时，也应导入为`processData`。基本上，即使JS允许不同的命名，也要保持命名的一致性。

**不好的做法：**
```ts
// ProcessDATA.ts
export default const processData = () => {
	// ...
};

// foo.ts
import doDataProcessing from './ProcessDATA';

doDataProcessing();
...
```

**好的做法：**
```ts
// processData.ts
export default const processData = () => {
	// ...
};

// foo.ts
import processData from './processData';

processData();
...
```

### 只导入您需要的内容

只导入您需要的内容，这样我们可以在将来实现[tree shaking](https://webpack.js.org/guides/tree-shaking/)时可能受益。

**不好的做法：**
```ts
import * as fs from 'fs-extra';
// ...
fs.writeFile('example.md', 'example');
```

**好的做法：**
```ts
import { writeFile } from 'fs-extra';
// ...
writeFile('example.md', 'example');
```

## 变量和函数

### 在新代码中对`const`使用`camelCase`

**不好的做法：**
```ts
// 不好！不要在新代码中使用！
const GRAVITY_ACCEL = 9.8;
```

**好的做法：**
```ts
const gravityAccel = 9.8;
```

### 在使用前声明变量

**不好的做法：**
```ts
// 不好！
let foo, bar;

const doThings = () => {
	// 做与foo、bar无关的事情
};

// 做涉及foo和bar的事情
foo = Math.random();
bar = foo + Math.random() / 100;
foo += Math.sin(bar + Math.tan(foo));
...
```

**好的做法：**
```ts
...
const doThings = () => {
	// 做与foo、bar无关的事情
};

// 做涉及foo和bar的事情
let foo = Math.random();
let bar = foo + Math.random() / 100;
foo += Math.sin(bar + Math.tan(foo));
...
```

但是，不要让这导致代码重复。如果常量在多个地方使用，可以在文件顶部声明它们，或者在单独的导入文件中声明。

### 尽可能优先使用`const`而不是`let`

### 优先使用`() => {}`而不是`function() { ... }`

这样做可以避免处理`this`关键字。没有它使得将类组件重构为React Hooks更容易，因为TypeScript将正确检测到任何使用`this`（在类中使用）的无效情况。

**不好的做法：**
```ts
// 不好！
function foo() {
	...
}
```

**好的做法：**
```ts
const foo = () => {
	...
};
```

另请参阅：[Frontend Armory — 何时应该在React中使用箭头函数？](https://frontarm.com/james-k-nelson/when-to-use-arrow-functions/)

### 避免默认参数和可选参数

尽可能避免在**函数定义**中使用默认参数和在**接口定义**中使用可选字段。当所有参数都是必需的时，重构代码会容易得多，因为编译器会自动捕获任何缺少的参数。

## 变量转义

[XSS](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)是当今代码中最常见的漏洞之一。这些漏洞通常很难发现，因为它们不是错误，它们通常不会导致任何测试单元失败，并且程序在99%的输入下都能正常工作。然而，剩余的1%可能被利用并用于窃取用户信息、使应用崩溃等。

如果您在Joplin的git日志中搜索["XSS"](https://github.com/laurent22/joplin/search?q=xss&type=commits)，您会发现过去几年已修复的几个安全漏洞，这些漏洞发生在各种难以预测的地方。因此，我们需要谨慎对待这个问题，并确保我们正确地转义用户内容。

即使我们认为我们控制了输入或者它总是具有某种格式，我们也应该这样做。这可能会在将来发生变化，或者可能通过另一个错误被利用。

最后，转义数据通常是防止标记代码崩溃所必需的。例如，引号或尖括号必须在HTML中转义，否则标记可能会崩溃。

如何转义数据取决于您要将其插入的位置，因此没有单一的函数可以覆盖所有情况。

### 插入到JS脚本中

使用`JSON.stringify()`。例如：

```ts
const jsCode = `const data = ${JSON.stringify(dynamicallyGeneratedData)};`
```

### 插入到HTML字符串中

您需要将特殊字符转换为HTML实体，我们通常使用`html-entities`包来完成这个工作。例如：

```ts
// 历史上，我们使用了PHP `htmlentities`函数的转换，因此名称不寻常（非驼峰式），
// 但由于很多代码都使用了该函数，我们保持这种方式。
import { htmlentities } from '@joplin/utils/html';
const html = `<a href="${htmlentities(attributes)}">${htmlentities(content)}</a>`;
```

### 插入到URL中

这取决于您要做什么。要插入查询参数，请使用`encodeURIComponent`

```ts
const url = `https://example.com/?page=${encodeURIComponent(page)}`;
``` 