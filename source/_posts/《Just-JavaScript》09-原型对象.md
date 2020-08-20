---
title: 《Just-JavaScript》09-原型对象
date: 2020-08-08 11:29:06
tags:
---

在前面的模块中，我们讨论了对象、属性和变异。但我们还没有完全讨论对象！

下面是一个小谜语来检验我们的思维模型：

```js
let pizza = {};
console.log(pizza.taste); // "pineapple"
```

问你自己：这可能吗？

我们刚刚用`{}`创建了一个空对象。在日志记录之前，我们绝对没有设置任何属性。所以看起来`pizza.taste`不能向`pineapple`。我们会料到的是`pizza.taste`给我们一个`undefined`。（当一个属性不存在时，我们通常无法得到`undefined`，对吗？）

我们可以在这两行之前添加一些代码让`pizza.taste`为`pineapple`！这可能是一个人为的例子，但它表明我们对JavaScript宇宙的思维模型是不完整的。

在本模块中，我们将介绍原型的概念。原型解释了在这个谜题中发生了什么。更重要的是，原型是其他几个JavaScript特性的核心。有时人们会因为它们看起来太不寻常而忽视学习。然而，核心思想非常简单。

# 原型

这里有几个变量指向几个对象：

```js
let human = {
  teeth: 32
};

let gwen = {
  age: 19
};
```

我们可以用熟悉的方式直观地表示它们：

![](/blog_imgs/just_javascript/09/example.png)

在本例中，`gwen`指向一个没有`teeth`属性的对象。根据我们所学的规则，如果我们读到它，我们就会得到`undefined`：

```js
console.log(gwen.teeth); // undefined
```

但故事不必就此结束。我们可以指示JavaScript继续搜索另一个对象上丢失的属性，而不是返回未定义的默认行为。我们只需一行代码就可以做到：

```js
let human = {
  teeth: 32
};

let gwen = {
  // We added this line:
  __proto__: human,
  age: 19
};
```

那神秘的`__proto__`是什么？

它代表了JavaScript原型的概念。任何JavaScript对象都可以选择另一个对象作为原型。我们将很快讨论这在实践中意味着什么。现在，让我们把它看作是一种特殊的`__proto__`导线：

![](/blog_imgs/just_javascript/09/proto.png)

花点时间验证图表是否与代码匹配。我们像以前一样画出它。唯一的新东西就是神秘的`__proto__`导线。

通过指定`__proto__`（也称为对象的原型），我们指示JavaScript继续查找该对象上缺少的属性。

## 使用原型

早些时候，当我们去寻找`gwen.teeth`。我们得到的是`undefined`，因为在gwen指向的对象上不存在`teeth`属性。

但多亏了这一点`__proto__`：人为的指向，现在答案不同了：


```js
let human = {
  teeth: 32
};

let gwen = {
  // "Look for other properties here"
  __proto__: human,
  age: 19
};

console.log(gwen.teeth); // 32
```

现在步骤顺序如下：

![](/blog_imgs/just_javascript/09/proto.gif)

1. 遵循`gwen`导线。它指向一个对象。
2. 这个对象有`teeth`属性吗？
    - 没有
    - **但它有一个原型**。我们来看看吧
3. 那个对象有`teeth`属性吗？
    - 是的，它指向32。
    - 因此，`gwen.teeth`的结果是32。

这和你在工作中可能会说：“我不知道，但爱丽丝可能知道”相似。使用`__proto__`，可以指示JavaScript去“询问另一个对象”。

要检查你目前的理解情况，请写下你的答案：

```js
let human = {
  teeth: 32
};

let gwen = {
  __proto__: human,
  age: 19
};

console.log(human.age); // ?
console.log(gwen.age); // ?

console.log(human.teeth); // ?
console.log(gwen.teeth); // ?

console.log(human.tail); // ?
console.log(gwen.tail); // ?
```

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你写完六个问题的答案之前不要再滚动。。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

现在让我们检查一下你的答案。

变量`human`指向的对象没有属性`age`，所以`human.age`是`undefined`。变量`gwen`指向具有age属性的对象。这条线指向19，所以`gwen.age`是19。

```js
console.log(human.age); // undefined
console.log(gwen.age); // 19
```

变量`human`指向具有属性`teeth`的对象，因此`human.teeth`是32。变量`gwen`指向一个没有属性`teeth`的对象。但是，该对象有一个原型，它确实具有`teeth`属性。这就是为什么`gwen.teeth`也是32。

```js
console.log(human.teeth); // 32
console.log(gwen.teeth); // 32
```

我们的两个对象都没有tail属性，因此这两个对象都未定义：

```js
console.log(human.tail); // undefined
console.log(gwen.tail); // undefined
```

注意，虽然`gwen.teeth`是32，这并不意味着`gwen`有属性`teeth`！实际上，在这个例子中，`gwen`没有`teeth`属性。但它的原型对象——`human`指向的对象确有。

这提醒我们`gwen.teeth`是一个表达式——对JavaScript宇宙的一个问题——JavaScript将遵循一系列步骤来回答它。现在我们知道这些步骤包括查看原型。

## 原型链

在JavaScript中，原型不是一个特殊的“东西”。原型更像是一种关系。一个对象可以指向另一个对象作为它的原型。

这自然会引出一个问题：但是如果我的对象的原型有自己的原型呢？那个原型有它自己的原型吗？这怎么作用呢？

答案正是它的工作原理！

```js
let mammal = {
  brainy: true,
};

let human = {
  __proto__: mammal,
  teeth: 32
};

let gwen = {
  __proto__: human,
  age: 19
};

console.log(gwen.brainy); // true
```

我们可以看到JavaScript将会搜索对象上的属性，然后搜索其原型，然后搜索该对象的原型，依此类推。如果我们查找完了原型，还没有找到我们的属性，我们就会得到`undefined`。

![](/blog_imgs/just_javascript/09/proto2.png)

这和你在工作中可能会说：“我不知道，但爱丽丝可能知道”。但爱丽丝可能会说“其实我也不知道，问问鲍勃”。最终，你要么得到答案，要么就没人可问了！

这个要“访问”的对象序列称为对象的原型链。（然而，与你可能戴的链不同，原型链不能是圆形的！）

## 跟踪

考虑一下这个稍加修改的例子：

```js
let human = {
  teeth: 32
};

let gwen = {
  __proto__: human,
  // This object has its own teeth property:
  teeth: 31
};
```

两个对象都定义了一个名为`teeth`的属性，因此结果不同：

```js
console.log(human.teeth); // 32
console.log(gwen.teeth); // 31
```

请注意`gwen.teeth`是31。如果`gwen`没有自己的`teeth`属性，我们会看看原型。但是因为`gwen`所指的对象有它自己的`teeth`属性，所以我们不需要一直寻找答案。

![](/blog_imgs/just_javascript/09/proto3.png)

换句话说，一旦我们找到我们的属性，**我们就停止搜索**。

如果你想检查一个对象自身是否有某个属性，你可以调用一个名为hasOwnProperty的内置函数。对于“own”属性，它返回true，并且不查看原型。在上一个示例中，两个对象都有自己的`teeth`指向，因此这两个对象都适用：

```js
console.log(human.hasOwnProperty('teeth')); // true
console.log(gwen.hasOwnProperty('teeth')); // true
```

## 赋值

考虑这个例子：

```js
let human = {
  teeth: 32
};

let gwen = {
  __proto__: human,
  // Note: no own teeth property
};

gwen.teeth = 31;

console.log(human.teeth); // ?
console.log(gwen.teeth); // ?
```

在赋值之前，两个表达式的结果都是32：

![](/blog_imgs/just_javascript/09/proto4.png)

然后我们需要执行这个任务：

```js
gwen.teeth = 31;
```

现在的问题是哪根导线对`gwen.teeth`起了作用？答案一般来说，是发生在对象本身上的赋值。

所以`gwen.teeth=31`是在gwen指向的对象上创建一个名为`teeth`的新的自有属性。它对原型没有任何影响：

![](/blog_imgs/just_javascript/09/proto5.png)

因此，`human.teeth`还是32，但是`gwen.teeth`现在31：

```js
console.log(human.teeth); // 32
console.log(gwen.teeth); // 31
```

我们可以用一个简单的经验法则来总结这种行为。

当我们读取对象上不存在的属性时，我们将继续在原型链上查找它。如果我们找不到它，我们就得到`undefined`。

但是当我们写一个在我们的对象上不存在的属性时，它会在我们的对象上创建这个属性。一般来说，原型不会起作用。

## 对象原型

这个对象没有原型，对吧？

```js
let obj = {};
```

请尝试在浏览器的控制台中运行：

```js
let obj = {};
console.log(obj.__proto__); // Play with it!
```

令人惊讶的是，`obj.__proto__`不为`null`或`undefined`！相反，你将看到一个具有一系列属性的奇怪对象，包括hasOwnProperty。

我们将这个特殊的对象称为对象原型：

![](/blog_imgs/just_javascript/09/proto6.png)

一开始，这可能有点令人费解。让我们充分理解它。我们一直认为`{}`创建了一个“空”对象。但不是空的！它有一个隐藏的`__proto__`线，默认情况下指向对象原型。

这解释了为什么JavaScript对象似乎具有“内置”属性：

```js
let human = {
  teeth: 32
};
console.log(human.hasOwnProperty); // function hasOwnProperty() { }
console.log(human.toString); // function toString() { }
```

这些“内置”属性只不过是对象原型上存在的普通属性。我们对象的原型就是对象原型，这就是为什么我们可以访问它们。（它们的实现在JS引擎中。）

## 没有原型的对象

我们刚刚了解到，所有使用`{}`语法创建的对象都有一个默认的对象原型，即特殊的`{}`原型。但我们也知道我们可以定制。你可能会想：我们能把它设为`null`吗？

```js
let weirdo = {
  __proto__: null
};
```

答案是肯定的_这将产生一个根本没有原型的对象。因此，它甚至没有内置的对象方法：

```js
console.log(weirdo.hasOwnProperty); // undefined
console.log(weirdo.toString); // undefined
```

就算可以，您通常不想创建这样的对象。然而，对象原型本身就是这样一个对象。它是一个没有原型的对象。

## 原型污染

现在我们知道，默认情况下，所有JavaScript对象都获得相同的原型。让我们简单回顾一下突变模块中的示例：

![](/blog_imgs/just_javascript/09/proto7.png)

这幅画给了我们一个有趣的见解。如果JavaScript在原型上搜索缺失的属性，而大多数对象共享同一个原型，我们是否可以通过改变原型使新属性“出现”在所有对象上？

答案是肯定的！

让我们添加这两行代码：

```js
let obj = {};
obj.__proto__.smell = 'banana';
```

我们通过添加一个`smell`属性来改变对象原型。因此，两位侦探现在似乎都在使用香蕉味的香水：

```js
console.log(sherlock.smell); // "banana"
console.log(watson.smell); // "banana"
```

![](/blog_imgs/just_javascript/09/proto8.png)

像我们刚才那样对共享原型进行改变称为原型污染。

在过去，原型污染是用自定义特性扩展JavaScript的流行方法。然而，多年来，web社区意识到它是脆弱的，很难添加新的语言特性。宁愿避免这样操作。

现在你可以从本单元开始解决`Pineapple Pizza`之谜了！在DevTools中检查您的解决方案。

# __proto__ 和 prototype

你可能会想知道：原型属性到底是什么？你可能已经在文档中看到了原型，例如在MDN页面标题中。

我有一个坏消息：`prototype`属性几乎与原型的核心机制完全无关（正如您可能还记得的那样，它是`__proto__`）。

原型属性主要与解释新操作符有关。我相信这一个不幸的命名选择是为什么这么多人被原型迷惑而放弃学习它们的主要原因。

## 为什么这很重要？

你可能会想：为什么要关心原型呢？你会经常用吗？实际上，您可能不会直接使用它们。不要养成写`__proto__`的习惯。这些例子只说明了机制。（事实上，即使直接使用`__proto__`语法本身也是[不可取](https://2ality.com/2015/09/proto-es6.html?ck_subscriber_id=767004595)的。）

原型有点不寻常，大多数人和框架从来没有真正将它们作为一个范例完全接受。相反，人们通常将原型仅仅用作传统“类继承”模型的构建块，这种模型在其他编程语言中很流行。事实上，JavaScript添加了一个`class`语法作为一种约定，将原型“隐藏”在视线之外。

不过，您会注意到原型隐藏在类和其他JavaScript特性的“表面之下”。例如，下面是一个JavaScript类的[片段](https://gist.github.com/gaearon/a25fd42a1e6b4cc24851978df0a36571?ck_subscriber_id=767004595)，它用`__proto__`重写，以演示幕后发生的事情。

就我个人而言，我在日常编码中使用的类不多，也很少直接处理原型。但是，了解这些特性是如何相互构建的，以及当我读取或设置对象的属性时会发生什么情况，这会有帮助。

# 总结

+ 当我们读取`obj.prop`，如果`obj`没有`prop`属性，JavaScript将查找`obj.__proto__.prop`，然后查找`obj.__proto__.__proto__.prop`，依此类推，直到找到我们的属性或到达原型链的末尾。

+ 写`obj.prop`的时候，JavaScript通常会直接写入对象，而不是遍历原型链。

+ 我们可以利用`obj.hasOwnProperty('prop')`来确定我们的对象自身是否有一个名为prop的属性。换句话说，这意味着有一个名为prop的属性线直接连接到该对象。

+ 我们可以通过改变原型来“污染”多个对象共享的原型。我们甚至可以对对象原型做这个操作——修改`{}`对象的默认原型！但我们不应该这样做，除非我们在和同事开玩笑。

+ 你可能不会在实践中直接使用原型。然而，它们是JavaScript对象如何工作的基础，因此理解它们的底层机制很方便。一些高级JavaScript特性，包括类，可以用原型来表示。

# 练习 

本模块还有习题供你练习！

[点击这里用一些简短的练习巩固这个心智模型。](https://eggheadio.typeform.com/to/S7p8NB?)

**不要跳过它们！**

尽管你可能对原型的概念很熟悉，但这些练习将帮助你巩固我们正在建立的思维模型。我们需要这个基础才能得到更复杂的话题。















