---
title: 《Just JavaScript》06. 值相等
date: 2020-06-09 17:17:27
tags:
---

是时候讨论JavaScript中的相等了，这就是为什么它很重要。

想象一下在一个戴面具的狂欢节上谈生意。你可能会和两个人说话，却没有意识到你其实和同一个人说了两次话。或者你可能以为你只和一个人谈过，但其实那是两个不同的人！

**如果你在JavaScript中没有一个清晰的关于相等的思维模型，name每一天都像一场狂欢——而且不是以一种好的方式。** 你永远不确定你是在处理同一个值，还是在处理两个不同的值。因此，你经常会犯错误——比如改变你不想改变的
值。

幸运的是，我们已经完成了在JavaScript中建立相等概念的大部分工作。它以一种非常自然的方式融入我们的思维模型。

# 相等的不同类型

![](/blog_imgs/just_javascript/06/mask.png)

在JavaScript中，有几种等式。如果你已经编写了一段时间的JavaScript，那么你可能至少熟悉其中的两个：

- **严格相等**：a === b (三等号).
- **松散相等**：a == b (双等号).
- **同值相等**：`Object.is(a, b)`.

大多数教程根本没有提到相同的值相等。我们要走一条人迹罕至的路，先解释一下它。我们可以用它来解释其他类型的相等。

## 同值相等: `Object.is(a, b)`

在JavaScript中，`Object.is(a, b)` 告诉我们a和b是否相同：

```
console.log(Object.is(2, 2)); // true
console.log(Object.is({}, {})); // false
```

这叫做同值相等。

在我们的思维模型中，“同样的值”到底意味着什么？你可能已经凭直觉知道了这是什么意思，但是让我们验证你的理解。

### 检验你的直觉

从下面计算值的练习中思考一下：

```javascript
let dwarves = 7;
let continents = '7';
let worldWonders = 3 + 4;
```

提醒一下，这个片段的草图如下所示：

![](/blog_imgs/just_javascript/06/sketch.png)

现在尝试使用上图回答这些问题：

```js
console.log(Object.is(dwarves, continents)); // ? f
console.log(Object.is(continents, worldWonders)); // ? f
console.log(Object.is(worldWonders, dwarves)); // ? t
```

写下你的答案，思考你会怎么解释。

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你写完之前不要再滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 
`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

这不是刁钻的问题！答案如下：

1. `Object.is(dwarves, continents)`是错误的，因为dwarves和continents指向不同的值。
2. `Object.is(continents, worldWonders)`是错误的，因为continents和worldWonders指向不同的值。
3. `Object.is(worldWonders, dwarves)` 是正确的，因为worldWonders和dwarves指向同一个值。

如果两个值在我们的图表中由一个形状表示，这意味着它们实际上不是两个不同的值。它们是同一个值！在这种情况下对象是`Object.is(a，b)`返回`true`。

在前面的模块中，我们“计算”了这些值。但实际上，我们正在学习什么使值彼此不同。结果，我们也学到了相反的东西——值相同意味着什么。

如果你在这个结论上还有困难，你可能会需要重新复习《计算数值》，并[再次完成练习](https://eggheadio.typeform.com/to/STEeMy?email=akihi95@gmail.com&ck_subscriber_id=767004595)。我保证这会有意义的！

### 但是关于对象呢？

此时，你可能会想到对象。你可能听说过，等式不适用于对象，它比较的是“引用”。**如果你有这样的直觉，先暂时把它们完全放在一边。**

相反，请看下面的代码片段：

```js
let banana = {};
let cherry = banana;
let chocolate = cherry;
cherry = {};
```

打开记事本或[草图应用程序](https://excalidraw.com/?ck_subscriber_id=767004595)，绘制变量和值的关系图。你应该一步一步画出来，因为这在你的脑子里很难做到。

记住`{}`总是意味着“创建一个新的对象值”。还要记住=表示“将左侧的导线连接到右侧的值”。

画完图后，对这些问题，写下你的答案：

```js
console.log(Object.is(banana, cherry)); // ? f
console.log(Object.is(cherry, chocolate)); // ? f
console.log(Object.is(chocolate, banana)); // ? t
```

一定要用你的图来回答他们。

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你画完图并写下答案之前不要往下滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 
`... ` 

`... ` 

`... ` 

`... ` 

绘图过程应遵循以下步骤：

![](/blog_imgs/just_javascript/06/sketch.gif)

1. `let banana = {};` 
    + 声明一个banana变量 
    + 创建新的对象值{}
    + 把banana变量的导线指向它

2. `let cherry = banana;`
    + 声明一个cherry变量
    + 把cherry的导线指到banana指的地方

3. `let chocolate = cherry;`
    + 接下来，我们声明一个香蕉变量
    + 把chocolate的导线指到cherry指的地方

4. `cherry = {};`
    + 新建一个对象{}
    + 把cherry的导线指向它

在最后一步之后，你的图应该如下所示：

![](/blog_imgs/just_javascript/06/wire.png)

现在让我们检查你的答案：

1. `Object.is(banana, cherry)`是**错误**的，因为banana和cherry指向**不同的值。**
2. `Object.is(cherry, chocolate)`是**错误**的，因为cherry和chocolate指向**不同的值。**
3. `Object.is(chocolate, banana)`是**正确**的，因为cherry和chocolate指向**同一个值。**

如你所见，我们不需要任何额外的概念来解释同值相等是如何对`objects`起作用的。它自然会从我们的思维模型中消失。

这就是我们要知道的一切！

## 严格相等：`a===b`

你以前可能使用过严格相等运算符：

```js
console.log(2 === 2); // true
console.log({} === {}); // false
```

也有对象相反的`!==`运算符。

### 同值相等与严格相等

那么`Object.is`和`===`有什么不同呢？

**同值相等**——`Object.is(a,b)`——在我们的思维模型中有直接的意义。它对应于我们宇宙中的“相同值”的概念。

在几乎所有的情况下，同样的直觉也适用于严格的值相等。例如，`2===2`是真的，因为2总是“召唤”相同的值：

![](/blog_imgs/just_javascript/06/two.gif)

相反，`{}=={}`为`false`，因为每个`{}`创建不同的值：

![](/blog_imgs/just_javascript/06/object.gif)

在上面的例子中，`a === b` 与`Object.is(a, b)`表现相同。然而，有**两种罕见的情况**下，`===`的行为是不同的。

**把下面的例子看作是规则的例外，**就像你在学习英语时必须记住不规则动词一样。这两个不寻常的案例都涉及我们过去讨论过的“特殊数字”。

1. `NaN === NaN` 是错误的，尽管它们是同样的值。
2. `-0 === 0` 和 `0 === -0` 是正确的，尽管它们是不同的值。

虽然这些情况并不常见，但我们将仔细研究这两个案例。

### 第一种特殊情况：`NaN`

正如我们在计算数值章节学到的的，NaN是一个特殊的数字，当我们进行0/0这样的无效数学运算时，它就会出现：

```js
let width = 0 / 0; // NaN
```

与NaN的进一步计算将再次给出NaN：

```js
let height = width * 2; // NaN
```

你可能不会故意这样做，但是当你一开始处理不正确的数据时，或者如果你的计算包含错误时，可能会发生这种情况

**记住`NaN===NaN`总是错误的：**

```js
console.log(width === height); // false
```

但是，NaN的值与NaN相同：

```js
console.log(Object.is(width, height)); // true
```

![](/blog_imgs/just_javascript/06/nan.png)

这太让人困惑了。

`NaN===NaN`是错误的，这在很大程度上是历史原因，所以我建议把它当作生活的一个事实。如果您试图编写一些代码来检查值是否为NaN（例如，打印警告），则可能会遇到这种情况。

```js
function resizeImage(size) {
  if (size === NaN) {
    // Doesn't work: the check is always false!
    console.log('Something is wrong.');
  }
  // ...
}
```

相反，这里有一些方法（它们都有效！）要检查`size`是否为NaN：

+ `Number.isNaN(size)`
+ `Object.is(size, NaN)`
+ `size !== size`

最后一个可能特别令人惊讶。腾出几分钟。如果你看不到它是如何检测到NaN的，试着重新阅读这一部分，然后再思考。

（本单元结束时会有答案。）

### 第二种特殊情况：`-0`

在常规数学中，没有“负零”这样的概念，但由于[实际原因](https://softwareengineering.stackexchange.com/questions/280648/why-is-negative-zero-important/280708#280708)，它存在于浮点数学中。这是一个有趣的事实。

**`0===-0`和`-0===0`始终为真：**

```js
let width = 0; // 0
let height = -width; // -0
console.log(width === height); // true
```

但是，0与-0是不同的值：

```js
console.log(Object.is(width, height)); // false
```

![](/blog_imgs/just_javascript/06/zero.png)

这也让人困惑。

实际上，在我的整个职业生涯中，我从来没有遇到过这种情况。

### 代码练习

现在你知道`Object.is`和`===`是怎么工作的，我有一个小的编码练习给你。你不一定非要完成它，但它是一个有趣的脑筋急转弯。

**编写一个名为strickequal(a,  b)的函数，该函数返回与`a===b`相同的值。您的实现不能使用===or！==运算符。**

如果你想检查一下自己的答案，这是[参考](https://gist.github.com/gaearon/08a85a33e3d08f3f2ca25fb17bd9d638?ck_subscriber_id=767004595)。这个函数是完全无用的，但是编写它有助于理解`===`。

### 不要惊慌

听到这些特殊的数字以及它们的行为会是我们无法抗拒的。不要对这些特殊情况过分强调。

它们不是很常见。既然你知道它们的存在，你就会在实践中认识到它们。在大多数情况下，对于判断`Object.is(a, b)`和`a === b`，我们对“同一个值”的直觉是很有用的。

## 松散相等

最后，轮到了最后一种相等的情况。

**松散相等**(double equals)是JavaScript的妖怪。

以下是几个让你抓狂的例子：

```js
console.log([[]] == ''); // true
console.log(true == [1]); // true
console.log(false == [0]); // true
```

[等等，这是什么？？？](https://dorey.github.io/JavaScript-Equality-Table/?ck_subscriber_id=767004595)

松散相等（也称为“抽象相等”）的规则既神秘又令人困惑。他们被广泛认为是一个早期糟糕的设计决策。许多编码标准完全禁止在代码中使用==和!=。

尽管JavaScript对应该或不应该使用哪些特性没有很强的见解，但我们现在不打算讨论**松散相等**。这在现代的代码库中是不常见的，对于语言或者我们的思维模型来说，它的规则并没有发挥重要的作用。如果你好奇，可以看看它是[如何工作](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness?ck_subscriber_id=767004595#Loose_equality_using)的，但不要觉得有压力去记住它。把记忆留给其他的话题。

有一种用法比较常见，值得了解：

```js
if (x == null) {
  // ...
}
```

此代码相当于编写：

```js
if (x === null || x === undefined) {
  // ...
}
```

然而，在某些团队中，即使使用==也可能会引起争议。作为一个团队，最好先讨论在代码库中允许多少==代码。

# 总结

+ JavaScript有几种等式。它们包括**同值相等**、**严格相等**和**松散相等**。
+ **同值相等**，或`Object.is(a, b)`，与我们在前面模块中介绍的值相同的概念相匹配。
    + 理解这种相等有助于防止错误！你经常需要知道什么时候你在处理同一个值，什么时候你在处理两个不同的值。
    + 当我们绘制一个值和变量的图表时，同一个值不能出现两次。当变量a和b在我们的图中指向同一个值时`Object.is(a, b)`返回真。
    + **同值相等**是最容易解释的，这就是为什么我们从它开始。但是，写起来冗长而且有点烦人。

+ 在实践中，你将经常使用**严格相等**，或`a===b`。除了两种罕见的特殊情况外，它相当于**同值相等**：
    + `NaN === NaN` 是错误的, 尽管它们是同样的值。
    + `-0 === 0` 和 `0 === -0` 是正确的，尽管它们是不同的值。
+ 你可以通过使用`Number.isNaN(x)`来检查x的值是否是NaN。
+ **松散相等**(==)是一组晦涩难懂的规则，通常可以避免。

最后，你可能还想知道为什么`size !== size`可以作为检测size是否为NaN的一种方式。我们说过我们会在本单元结束时重新讨论这个问题。这是因为`NaN === NaN`返回`false`，正如我们已经知道的。所以反过来`(NaN !== NaN)`必须为真。既然NaN是唯一不等于自身的值，那么`size !== size`只能表示size为NaN。

事实上，确保你可以通过这种方式检测NaN是使NaN===NaN返回为false的[原始原因之一](https://stackoverflow.com/questions/1565164/what-is-the-rationale-for-all-comparisons-returning-false-for-ieee754-nan-values/1573715#1573715)！这是在JavaScript出现之前决定的。这是一个纯粹的历史轶事，但还是很有趣。

# 练习

本单元也有练习题供你练习！

[点击这里，通过一些简短的练习巩固这个思维模型。](https://eggheadio.typeform.com/to/wpQKI4?email=akihi95@gmail.com&ck_subscriber_id=767004595)

**不要跳过它们！**

尽管你可能对相等的概念很熟悉，这些练习将帮助你巩固我们正在建立的思维模型。我们需要打好基础才能继续更复杂的话题。































