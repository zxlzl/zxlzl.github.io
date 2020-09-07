---
title: symbol
date: 2020-08-22 22:06:39
tags:
---

[toc]

# 你不知道的Symbol

> ES6中新加入的 原始数据类型(基本数据类型/值类型)，代表唯一值
> - 对象的唯一属性：防止同名属性，及被改写或覆盖
> - 消除魔术字符串：指代码中多次出现，强耦合的字符串或数值，应避免，而使用含义清晰的变量代替

```js
const key = Symbol(),
    key2 = Symbol();
let obj = {
    [key]: 'zxl'
};
console.log(obj[key]); //"zxl"
console.log(obj[key2]); //undefined

/*
// 魔术字符串：在代码中 多次出现、与代码形成强耦合的某一个具体的字符串或者数值。
function reducer(action) {
    let state = {
        count: 0
    };
    switch (action.type) {
        case "vote_plus": //魔术字符串
            state.count++;
            break;
    }
    return state;
}
reducer({
    type: 'vote_plus' //魔术字符串
}); 
*/

const vote_plus = Symbol("vote_plus");
function reducer(action) {
    let state = {
        count: 0
    };
    switch (action.type) {
        case vote_plus:
            state.count++;
            break;
    }
    return state;
}
reducer({
    type: vote_plus
});
```

## 基本语法
- Symbol() 和 Symbol([key])
- Symbol函数不能被new
- Symbol.prototype 与 Object(Symbol())
- Symbol不能与其他类型的值计算
  - 数学计算：不能转换为数字
  - 字符串拼接：隐式转换不可以，但是可以显示转换
  - 模板字符串
- Symbol 属性不参与 for…in/of 遍历
  - Object.getOwnPropertySymbols
  - Object.getOwnPropertyNames
  - JSON.stringify
  - Object.keys

## 内置的Symbol值

> ES6提供很多内置的Symbol值，指向语言内部使用的方法
> - Symbol.hasInstance：对象的Symbol.hasInstance属性，指向一个内部方法，当其他对象使用instanceof运算符，判断是否为该对象的实例时，会调用这个方法
> - Symbol.isConcatSpreadable：值为布尔值，表示该对象用于Array.prototype.concat()时，是否可以展开
> - Symbol.iterator：拥有此属性的对象被誉为可被迭代的对象，可以使用for…of循环
> -Symbol.toPrimitive: 该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值
> - Symbol.toStringTag：在该对象上面调用Object.prototype.toString方法时，如果这个属性存在，它的返回值会出现在toString方法返回的字符串之中，表示对象的类型

```js
class Person {}
let p1 = new Person;
console.log(p1 instanceof Person); //true
console.log(Person[Symbol.hasInstance](p1)); //true
console.log(Object[Symbol.hasInstance]({})); //true
```

```js
let arr = [4, 5, 6];
console.log(arr[Symbol.isConcatSpreadable]); //undefined
let res = [1, 2, 3].concat(arr);
console.log(res); //[1,2,3,4,5,6]

arr[Symbol.isConcatSpreadable] = false;
res = [1, 2, 3].concat(arr);
console.log(res); //[1,2,3,[4,5,6]]
```

```js
// 让对象变为可迭代的值
let obj = {
    0: 'zxl',
    1: 11,
    length: 2,
    [Symbol.iterator]: Array.prototype[Symbol.iterator]
};
for (let item of obj) {
    console.log(item); // zxl 11
} 

// 构造一个类数组
class ArrayLike {
    *[Symbol.iterator]() {
        let i = 0;
        while (this[i] !== undefined) {
            yield this[i];
            ++i;
        }
    }
}
let like1 = new ArrayLike;
like1[0] = 10;
like1[1] = 20;
for (let item of like1) {
    console.log(item);
}
```

```js
/*
 * 对象数据类型进行转换：
 * 1. 调用obj[Symbol.toPrimitive](hint)，前提是存在
 * 2. 否则，如果 hint 是 "string" —— 尝试 obj.toString() 和 obj.valueOf()
 * 3. 否则，如果 hint 是 "number" 或 "default" —— 尝试 obj.valueOf() 和 obj.toString()
 */
 
 let a = {
    value: 0,
    [Symbol.toPrimitive](hint) {
        switch (hint) {
            case 'number': //此时需要转换成数值 例如:数学运算`
                return ++this.value;
            case 'string': // 此时需要转换成字符串 例如:字符串拼接
                return String(this.value);
            case 'default': //此时可以转换成数值或字符串 例如：==比较
                return ++this.value;
        }
    }
};
if (a == 1 && a == 2 && a == 3) {
    console.log('OK');
}
```
 
```js
class Person {
    get [Symbol.toStringTag]() {
        return 'Person';
    }
}
let p1 = new Person;
console.log(Object.prototype.toString.call(p1)); //"[object Person]"
```

## Generator生成器/Iterator迭代器及实战中的运用

>Iterator 是 ES6 引入的一种新的遍历机制
> - 通过 Symbol.iterator 创建一个迭代器，指向当前数据结构的起始位置
> - 随后通过 next 方法进行向下迭代指向下一个位置， next 方法会返回当前位置的对象，对象包含了 value 和 done 两个属性， value 是当前属性的值， done 用于判断是否遍历结束
> - 当 done 为 true 时则遍历结束

```js
// 创造Iterator
function createIterator(items) {
    let i = 0;
    return {
        next: function () {
            var done = (i >= items.length);
            var value = !done ? items[i++] : undefined;
            return {
                done: done,
                value: value
            };
        }
    };
}

// 迭代器特点
const arr = [10, 20, 30];
const it = arr[Symbol.iterator]();
console.log(it.next()); //{value: 10, done: false}
console.log(it.next()); //{value: 20, done: false}
console.log(it.next()); //{value: 30, done: false}
console.log(it.next()); //{value: undefined, done: true}
```

> Generator生成器对象是由一个 generator function 返回的,并且它符合可迭代协议

```js
function* gen() {
    yield 1;
    yield 2;
    yield 3;
    return 4;
}
let g = gen();
/* console.log(g.next()); //{value:1,done:false}
console.log(g.next()); //{value:2,done:false}
console.log(g.next()); //{value:3,done:false}
console.log(g.next()); //{value:4,done:true}
console.log(g.next()); //{value:undefined,done:true} */

/*
let item = g.next(),
  arr = [];
arr.push(item.value);
while (!item.done) {
  item = g.next();
  arr.push(item.value);
}
console.log(arr);
*/

let arr = [];
for (let item of g) {
    arr.push(item);
}
console.log(arr); //[1,2,3]
console.log(g.next()); //{value:undefined,done:true}
```

```js
function* gen() {
    console.log('OK');
    let x = yield 1;
    console.log(x);
    yield 2;
}
let g = gen();
console.log(g.next());
console.log(g.next(10));
```

<span style="color:red">`yield 委托*`</span>  
在Generator函数中，我们有时需要将多个迭代器的值合在一起，我们可以使用yield *的形式，将执行委托给另外一个Generator函数
```js
function* foo1() {
    yield 1;
    yield 2;
}
function* foo2() {
    yield 3;
    yield 4;
}
function* foo() {
    yield* foo1();
    yield* foo2();
    yield 5;
}
const result = foo();
console.log(result.next()); // { value: 1, done: false }
console.log(result.next()); // { value: 2, done: false }
console.log(result.next()); // { value: 3, done: false }
console.log(result.next()); // { value: 4, done: false }
console.log(result.next()); // { value: 5, done: false }
console.log(result.next()); // { value: undefined, done: true }
```

## 手撕async/await源码
```js
// 模拟API请求接口
function API(num) {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve(num);
        }, 1000);
    });
}

//自动执行器，如果一个Generator函数没有执行完，则递归调用
function asyncFun(generator) {
    let iterator = generator();

    function next(data) {
        let result = iterator.next(data);
        if (result.done) return result.value;
        result.value.then(function (data) {
            next(data);
        });
    }
    next();
}

asyncFun(function* () {
    let res1 = yield API(10);
    console.log(res1);
    let res2 = yield API(res1 + 10);
    console.log(res2);
});
```

### Set/Map运用
> ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。
> - 基础语法
> - Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality [iˈkwɒləti] ”，它类似于 ===，主要的区别是它认为NaN和NaN相等的

> WeakSet 结构与 Set 类似，也是不重复的值的集合，但是，它与 Set 有两个区别：
> - 首先，WeakSet 的成员只能是对象，而不能是其他类型的值
> - 其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中

```js
 let a = new Set([1, 2, 3]);
 let b = new Set([2, 3, 4]);

 // 并集
 let union = new Set([...a, ...b]);

 // 交集
 let intersect = new Set([...a].filter(x => b.has(x)));

 // 差集
 let difference = new Set([...a].filter(x => !b.has(x)));
 ```
 
 > ES6 提供了 Map 数据结构，它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键

