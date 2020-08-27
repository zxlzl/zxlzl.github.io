---
title: 《typescript》01-环境搭建与编译
date: 2020-08-27 11:07:08
tags:
---



# TypeScript - 环境搭建与编译

## TypeScript 环境搭建

<u>TypeScript</u> 编写的程序并不能直接通过浏览器运行，我们需要先通过 <u>TypeScript</u> 编译器把 <u>TypeScript</u> 代码编译成 <u>JavaScript</u> 代码

<u>TypeScript</u> 的编译器是基于 <u>Node.js</u> 的，所以我们需要先安装 <u>Node.js</u>

**Node.js 安装**

[https://nodejs.org](https://nodejs.org/)

安装完成以后，通过 终端或者 cmd 等命令行工具来调用 node

```bash
node -v
```

**通过 NPM 包管理工具安装 TypeScript 编译器**

https://www.npmjs.com/

http://www.typescriptlang.org/

```bash
npm install -g typescript
```

<u>TypeScript 编译器</u> 安装成功以后，会提供一个 <u>tsc</u> 的命令，用于编译我们的 <u>TypeScript</u> 代码文件

```bash
tsc -v
```

## TypeScript 编译

**TypeScript 代码**

```ts
let str: string = 'zxl';
```

**编译 TypeScript 代码**

```shell
tsc <要编译的ts文件>
```

默认情况下会在当前文件所在目录下生成同名的 js 文件

### 编译选项

编译命令 <u>tsc</u> 还支持许多编译选项。

**--outDir**

指定编译文件输出目录

```shell
tsc --outDir ./dist ./src/HelloTypeScript.ts
```

**--target**

指定编译的代码版本目标，默认为 <u>ES3</u>

```shell
tsc --outDir ./dist --target ES6 ./src/HelloTypeScript.ts
```

**--watch**

在监听模式下运行，当文件发生改变的时候自动编译

```shell
tsc --outDir ./dist --target ES6 --watch ./src/HelloTypeScript.ts
```

通过上面几个例子，基本可以了解 <u>tsc</u> 的使用了，如果每次编译都输入这么一大堆的选项是真的很繁琐。

其实，<u>TypeScript</u> 编译为我们提供了一个更加强大且方便的方式，编译配置文件：<u>tsconfig.json</u>，我们可以把上面的编译选项保存到这个配置文件中

### 编译配置文件

```json
{
	"compilerOptions": {
		"outDir": "./dist",
		"target": "ES2015",
    "watch": true,
	},
  "include": ["./src/**/*"]
}
```

**include**

指定需要编译的 <u>ts</u> 文件目录，如果没有指定，则默认包含当前目录及子目录下的所有 <u>ts</u> 文件

**默认配置**

<u>tsc</u> 默认会从当前目录开始去查找 <u>tsconfig.json</u> 文件，如果没有找到，会逐级向上搜索父目录

```shell
tsc
```

**指定配置文件**

使用 <u>--project</u> 或 <u>-p</u> 也可以指定某个具体的配置文件

```shell
tsc -p ./c.json
```

**指定配置文件目录**

使用 <u>-p</u> 指定配置文件所在目录，<u>tsc</u> 会默认加载该目录下的 <u>tsconfig.json</u> 文件

```shell
tsc -p ./src
```

除了以上配置，<u>ts</u> 的编译配置选项还有很多。

更多编译选项：

> http://www.typescriptlang.org/docs/handbook/tsconfig-json.html
>
> http://www.typescriptlang.org/docs/handbook/compiler-options.html



## 总结

- 环境搭建
- 编译命令与配置
- 配置文件：<u>tsconfig.json</u>
  - outDir、target、watch、include、project

