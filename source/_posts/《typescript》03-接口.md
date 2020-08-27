---
title: 《typescript》03-接口
date: 2020-08-11 23:43:48
tags:
---

# TypeScript - 接口（ Interface）

## 接口

<u>TypeScript</u> 的核心之一就是对值（数据）所具有的结构进行类型检查，除了一些前面说到基本类型标注，针对对象类型的数据，除了前面提到的一些方式意外，我们还可以通过： <u>Interface （接口）</u>，来进行标注。

<u>接口</u>：对复杂的对象类型进行标注的一种方式，或者给其它代码定义一种契约（比如：类）

接口的基础语法定义结构特别简单

```typescript
interface Point {
    x: number;
    y: number;
}
```

上面的代码定义了一个类型，该类型包含两个属性，一个 <u>number</u> 类型的 <u>x</u> 和一个 <u>number</u> 类型的 <u>y</u>，接口中多个属性之间可以使用 <u>逗号</u> 或者 <u>分号</u> 进行分隔

我们可以通过这个接口来给一个数据进行类型标注

```typescript
let p1: Point = {
    x: 100,
    y: 100
};
```

> <span style="color:red">注意：接口是一种 <u>类型</u> ，不能作为 <u>值</u> 使用</span>

```typescript
interface Point {
    x: number;
    y: number;
}

let p1 = Point;	//错误
```

当然，接口的定义规则远远不止这些

### 可选属性


接口也可以定义可选的属性，通过 <u>?</u> 来进行标注

```typescript
interface Point {
    x: number;
    y: number;
    color?: string;
}
```

其中的 <u>color?</u> 表示该属性是可选的

### 只读属性

我们还可以通过 <u>readonly</u> 来标注属性为只读

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

当我们标注了一个属性为只读，那么该属性除了初始化以外，是不能被再次赋值的

### 任意属性

有的时候，我们希望给一个接口添加任意属性，可以通过索引类型来实现

**数字类型索引**

```typescript
interface Point {
    x: number;
    y: number;
    [prop: number]: number;
}
```

**字符串类型索引**

```typescript
interface Point {
    x: number;
    y: number;
    [prop: string]: number;
}
```

数字索引是字符串索引的子类型

**注意事项**

> <span style="color:red">索引签名参数类型必须为 <u>string</u> 或 <u>number</u> 之一，但两者可同时出现</span>

```typescript
interface Point {
    [prop1: string]: string;
    [prop2: number]: string;
}
```

> <span style="color:red">当同时存在数字类型索引和字符串类型索引的时候，数字类型的值类型必须是字符串类型的值类型或子类型</span>

```typescript
interface Point1 {
    [prop1: string]: string;
    [prop2: number]: number;	// 错误
}
interface Point2 {
    [prop1: string]: Object;
    [prop2: number]: Date;	// 正确
}
```

## 总结

- 接口语法与使用
- 可选属性
- 只读属性
- 任意属性 - 索引属性
  - 数字索引
  - 字符串索引



## 思考

- 为什么数字索引值的类型必须是字符串索引值的类型或其子类型？
  - 这是因为当使用 number来索引时，JavaScript会将它转换成string然后再去索引对象。 也就是说用 100（一个number）去索引等同于使用"100"（一个string）去索引，因此两者需要保持一致。