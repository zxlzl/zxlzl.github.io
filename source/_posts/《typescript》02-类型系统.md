---
title: 《typescript》02-类型系统
date: 2020-08-25 11:12:28
tags:
---

# TypeScript - 类型系统

## 类型系统

简单来说类型系统包含：

- 类型标注（签名）
- 类型检测

### 类型标注

类型标注就是给数据（变量、函数、类等）添加类型说明

类型标注语法：

变量: 标注类型

```typescript
let 变量: 数据类型;
```

### 类型检测

有了类型标注，编译器会在编译过程中根据标注的类型进行检测，使数据的使用更安全，帮助我们减少错误

同时配合 编辑器/IDE ，类型标注还能提供更加强大和友好的智能提示

> <span style="color:green">注意：类型系统检测的是类型，而不是具体值，即 <u>数据是否与标注的类型一致</u></span>

### 标注类型有哪些？

- 基础类型
- 空和未定义类型
- 对象类型
- 数组类型
- 元组类型
- 枚举类型
- 无值类型
- Never类型
- 任意类型
- 未知类型（Version3.0 Added）

#### 基础类型

基础类型包含：<u>string</u>，<u>number</u>，<u>boolean</u>

标注语法

```typescript
变量: string;
变量: number;
变量: boolean;
```

```typescript
let title: string = 'zxl';
let n: number = 100;
let isOk: boolean = true;

title = 100;	//错误
```

#### 空和未定义类型

因为在 <u>Null</u> 和 <u>Undefined</u> 这两种类型有且只有一个值，在标注一个变量为 <u>Null</u> 和 <u>Undefined</u> 类型，那就表示该变量不能修改了

默认情况下 <u>null</u> 和 <u>undefined</u> 是所有类型的子类型。 就是说你可以把 <u>null</u> 和 <u>undefined</u> 赋给其它类型的变量

如果一个变量声明了，但是未赋值，那么该变量的值为 undefined，但是如果它同时也没有标注类型的话，默认类型为 <u>any</u>

```typescript
let un: undefined;
un = 1;	// 错误
let nul: null;
nul = 1; //错误
let a: string = 'zxl';
a = null; // 可以
a = undefined; // 可以
```

> <span style="color:green">小技巧：指定 <u>strictNullChecks</u> 配置为 <u>true</u>，可以有效的检测 <u>null</u> 值数据，避免很多常见问题，建议对可能出现的 <u>null</u> 和 <u>undefined</u> 进行容错处理，使程序更加严谨</span>
>
> ```typescript
> let ele = document.querySelector('#box');
> if (ele) {
> 		ele.style.display = 'none';
> }
> ```

#### 对象类型

Object 类型表示非原始值类型

标注语法

```typescript
变量: object
```

**基于对象字面量的类型标注**

```typescript
let ot: {x: number, y: string} = {
    x: 1,
    y: 'zmouse'
};
```

针对对象这种特殊而有复杂的数据，<u>TypeScript</u> 有许多的方式来进行类型标注

**内置对象类型**

除了 Object 类型，在 JavaScript 中还有很多的内置对象，如：Date，标注如下：

```typescript
变量: 内置对象构造函数名
```

```typescript
let d1: Date = new Date();
let set1: Set = new Set();
```

**包装对象**

这里说的包装对象其实就是 <u>JavaScript</u> 中的 <u>String</u>、<u>Number</u>、<u>Boolean</u>，我们知道 <u>string</u> 类型 和 <u>String</u> 类型并不一样，在 <u>TypeScript</u> 中也是一样

```typescript
let a: string;
a = '1';
a = new String('1');	// 错误

let b: String;
b = new String('2');
b = '2';	// 正确
```

#### 数组类型

> <span style="color:green"><u>TypeScript</u> 中的数组存储的类型必须一致，所以在标注数组类型的时候，同时要标注数组中存储的数据类型</span>

标注语法

```typescript
变量: 类型[]
```

数组还有另外一种基于 泛型 的标注

变量: Array<类型>

```typescript
let arr1: string[] = [];
arr1.push('开课吧'); // 正确
arr1.push(1); // 错误

let arr2: Array<number> = [];
arr2.push(100); // 正确
arr2.push('开课吧'); // 错误
```

#### 元组类型

元组类似数组，但是存储的元素类型不必相同，但是需要注意：

> - <span style="color: green">初始化数据的个数以及对应位置标注类型必须一致</span>
> - <span style="color: green">越界数据必须是元组标注中的类型之一（标注越界数据可以不用对应顺序 - <u>联合类型</u>）</span>

标注语法

```typescript
变量: [类型一, 类型二,...]
```

```typescript
let data1: [string, number] = ['开课吧', 100];
data1.push(100); // 正确
data1.push('100'); // 正确
data1.push(true); // 错误
```

> <span style="color:green">注意：未开启 <u>strictNullChecks: true</u> 会使用 undefined 进行初始化</span>

#### 枚举类型

枚举的作用组织收集一组关联数据的方式

标注语法

```typescript
enum 枚举名称 { key1=value, key2=value2 }
```

- <u>key</u> 不能是数字
- <u>value</u> 可以是数字，称为 <u>数字类型枚举</u>，也可以是字符串，称为 <u>字符串类型枚举</u>，但不能是其它值，默认为数字：<u>0</u>
- 第一个枚举值或者前一个枚举值为数字时，可以省略赋值，其值为 <u>前一个数字值 + 1</u> 

数字类型枚举

```typescript
enum HTTP_CODE {
    OK = 200,
    NOT_FOUND = 404
};
HTTP_CODE.OK	//200
```

字符串类型枚举

```typescript
enum URLS  {
    USER_REGISETER = '/user/register',
    USER_LOGIN = '/user/login'
}
```

#### 无值类型

表示没有任何数据的类型，通常用于标注无返回值函数的返回值类型，函数默认标注类型位：<u>void</u>

标注语法

```typescript
变量: void
```

```typescript
function fn():void {
  	// 没有 return
}
```

#### Never类型

当一个函数永远不可能执行 <u>return</u> 的时候，返回的就是 <u>never</u> ，与 <u>void</u> 不同，<u>void</u> 是执行了 <u>return</u>， 只是没有值，<u>never</u> 是不会执行 <u>return</u>，比如抛出错误，导致函数终止执行

```typescript
function fn(): never {
  	throw new Error('error');
}
```

- <u>never</u> 类型是所有其它类型的子类
- 其它不能赋值给 <u>never</u> 类型，即使是 <u>any</u>

#### 任意类型

有的时候，我们并不确定这个值到底是什么类型或者不需要对该值进行类型检测，就可以标注为 any 类型

- 任何值都可以赋值给 <u>any</u> 类型
- <u>any</u> 类型也可以赋值给任意类型
- <u>any</u> 类型有任意属性和方法

标注语法

```typescript
变量: any
```

> <span style="color:green">注意：标注为 <u>any</u> 类型，也意味着放弃对该值的类型检测，同时放弃 IDE 的智能提示</span>

> <span style="color:green">小技巧：指定 <u>noImplicitAny</u> 配置为 <u>true</u>，当函数参数出现隐含的 <u>any</u> 类型时报错</span>

#### 未知类型

<u>unknow</u>，3.0 版本中新增，属于安全版的 <u>any</u>，但是与 any 不同的是：

- <u>unknow</u> 仅能赋值给 <u>unknow</u>、<u>any</u>
- <u>unknow</u> 没有任何属性和方法

标注语法

```typescript
变量: unknow;
```

#### 函数类型

在 <u>JavaScript</u> 中函数是一等公民的存在，在 <u>TypeScript</u> 也是如此，同样的，函数也有自己的类型标注格式

```typescript
函数名称( 参数1: 类型, 参数2: 类型... ): 返回值类型;
```

```typescript
function add(x: number, y: number): number {
  	return x + y;
}
```

## 总结

- 类型标注语法
- 基础类型标注
  - 字符串、数字、布尔值、空、未定义
- 非基础类型标注
  - 对象、数组
- 特殊类型
  - 元组、枚举、无值类型、Never类型、任意类型、未知类型
- 包装对象


