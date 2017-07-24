---
title: 【备忘】TypeScript中使用异步迭代器(Async Iterators)
date: 2017-06-16 19:32:10
urlname: use_async_iterators_in_typescript2.3.4
tags:
    - TypeScript
    - 异步迭代器
---

**本文没有任何技术含量**

`TypeScript`2.3新增了对异步迭代器的支持，但是在当前版本(2.3.4)中并未默认开启，直接使用会报以下错误:

```bash
error TS2318: Cannot find global type 'AsyncIterableIterator'. example.ts(10,26):

error TS2504: Type must have a 'Symbol.asyncIterator' method that returns an async iterator.
```

本文用于记录如何开启TypeScript对异步迭代器的支持
<!--more-->

### 1.安装core-js依赖

使用npm执行以下命令

```bash
npm install core-js --save
```

### 2. 配置注入的lib

在tsconfig.json-compilerOptions-lib中增加`esnext.asynciterable`

如果出现报错，可以继续增加`es2015`到lib中

示例:

```json
{
    "compilerOptions": {
        "target": "es2015",
        "module": "commonjs",
        "lib": [
        "esnext.asynciterable",
        "es2015"
        ]
    }
}

```
### 3. 导入async-iterator

在ts文件中加入以下语句:

```ts
import "core-js/modules/es7.symbol.async-iterator";
```

### 代码实例

```ts
import "core-js/modules/es7.symbol.async-iterator";

// await版sleep，在当前逻辑流上"休眠"timeout毫秒
const sleep = (timeout: number) => new Promise(r => setTimeout(() => r(), timeout));

// 一个异步生成器函数
async function* example_function() {
    for (let i = 0; i < 5; i++) {
        await sleep(1000);
        yield i;
    }
}

// 程序入口
(async function main() {
    for await (let i of example_function()) {
        console.log(i);
    }
})();
```