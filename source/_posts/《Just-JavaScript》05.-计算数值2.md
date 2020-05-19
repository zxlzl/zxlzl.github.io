---
title: 《Just JavaScript》05. 计算数值2
date: 2020-05-18 17:36:08  
categories: JavaScript
tags:

---

事不宜迟，让我们继续我们的JavaScript之旅吧！

![](/blog_imgs/just_javascript//05/type.png)

在前面的模块中，我们研究了undefined、null、boolean和number。我们现在将继续计算数值——从bigint开始。

## BigInts

![](/blog_imgs/just_javascript//05/bigints.png)

BigInts只是最近才添加到JavaScript中，所以你还不会看到它们被广泛使用。如果你使用版本较低的浏览器，它们将不起作用。常规数字不能精确地表示大整数，因此大整数填补了这一空白（字面上）：

``` javascript
let alot = 9007199254740991n; // Notice n at the end
console.log(alot + 1n); // 9007199254740992n
console.log(alot + 2n); // 9007199254740993n
console.log(alot + 3n); // 9007199254740994n
console.log(alot + 4n); // 9007199254740995n
console.log(alot + 5n); // 9007199254740996n
```

四舍五入可不是闹着玩的！这对于财务计算来说是非常重要的，因为精确性尤其重要。请记住，没有什么是免费的。真正数量庞大的操作可能需要时间和资源。

我们的宇宙中有多少个BigInts？明确地说它们有任意的精度。这意味着**在我们的JavaScript世界中，有无限多个BigInts——数学中每个整数对应一个。**

是吗？

如果这听起来很奇怪，考虑一下你已经习惯了数学中存在无限整数的想法。（如果不是，请稍等片刻！）从“数学世界”到“JavaScript世界”并不是什么飞跃。

（从那里，我们可以直接进入百事世界。）

当然，在实践中，我们不可能把所有可能的高精度计算放进计算机内存。如果我们尝试，在某些时候，它会崩溃或冻结。但从概念上讲，计算伯爵(个人理解指计算机程序)可能永远忙着计算，从未停止过。

## Strings

![](/blog_imgs/just_javascript//05/string.png)

在JavaScript用字符串表示文本。有三种方法可以可以写字符串（单引号、双引号和反引号），但结果是一样的：

``` javascript
console.log(typeof("こんにちは")); // "string"
console.log(typeof('こんにちは')); // "string"
console.log(typeof( `こんにちは` )); // "string"
```

空字符串也是字符串：

``` javascript
console.log(typeof('')); // "string"
```

### 字符串不是对象

所有字符串都有一些内置属性。

``` javascript
let cat = 'Cheshire';
console.log(cat.length); // 8
console.log(cat[0]); // "C"
console.log(cat[1]); // "h"
```

这并不意味着字符串就是对象！字符串属性是特殊的，其行为与对象属性不同。例如，不能将任何内容分配给cat[0]。字符串是原始值，所有原始值都是不可变的。

### 每个可能的字符串的值

**在我们的宇宙中，每个可能的字符串都有一个不同的值**。是的，这包括你祖母的娘家姓，十年前你用化名发表的同人小说，和还没有写完的《黑客帝国5》的剧本。

当然，所有可能的字符串都不能完全放在计算机内存芯片中。但是每一个可能的字符串都可以放在你的脑子里。我们的JavaScript宇宙是人类的模型，而不是计算机的模型！

这可能会引发一个问题。此代码是否创建字符串？

``` javascript
// Try it in your console
let answer = prompt('Enter your name');
console.log(answer); // ?
```

或者它只是召唤一个已经存在于我们宇宙中的字符串？

这个问题的答案取决于我们是“从外部”还是“从内部”学习JavaScript。

在我们的思维模型之外，答案取决于具体的实现。字符串是表示为单个内存块、多个块还是串在一起的相似的东西，取决于JavaScript引擎。

但在我们的思维模式中，这个问题并不意味着什么。我们不能建立一个实验来说明在我们的JavaScript宇宙中字符串是“被创建”还是“被调用”。

为了保持我们的思维模型简单，我们将**所有可能的字符串值从一开始就保存了它们——每个不同的字符串都有一个值。**

## Symbols

Symbols是最近才加到JavaScript中的。

```javascript
let alohomora = Symbol();
console.log(typeof(alohomora)); // "symbol"
```

如果不深入研究对象和属性，很难解释它们的目的和行为，所以现在我们将跳过它们。对不起，symbols！

![](/blog_imgs/just_javascript//05/sym.png)

## Objects

最后，我们来说说对象。

![](/blog_imgs/just_javascript//05/bracket.png)

对象包括arrays, dates, RegExps和其他非原始值。

```javascript
console.log(typeof({})); // "object"
console.log(typeof([])); // "object"
console.log(typeof(new Date())); // "object"
console.log(typeof(/\d+/)); // "object"
console.log(typeof(Math)); // "object"
```

与之前的一切不同，对象不是原始值。这也意味着默认情况下，它们是可变的。我们可以通过.或者[]访问他们的属性：

```javascript
let rapper = { name: 'Malicious' };
rapper.name = 'Malice'; // Dot notation
rapper['name'] = 'No Malice'; // Bracket notation
```

我们还没有详细讨论属性，所以你对它们的思维模型可能是模糊的。我们将在未来的模块中讨论属性。

### 创建我们自己的对象

有一件事特别使计算伯爵对对象象感到兴奋。**我们可以创建更多对象，我们可以创建自己的对象。**

在我们的思维模型中，我们讨论过的所有原始值——null, undefined, booleans, numbers和strings——塔门都“一直存在”。我们不能“制造”一个新字符串或一个新数字，我们只能“转换”那个值。

```javascript
let sisters = 3;
let musketeers = 3;
```

![](/blog_imgs/just_javascript//05/primitive.png)

使对象区别于其他的是我们可以创建更多的对象。每次使用{}对象文本时，我们都会创建一个全新的对象值：

```javascript
let shrek = {};
let donkey = {};
```

![](/blog_imgs/just_javascript//05/obj.png)

数组、日期和任何其他对象也是如此。例如，[]数组确实创建了一个新的数组值——以前从未存在过的值。

### 对象消失了吗？

你可能会想：对象会永远消失，还是永远在周围徘徊？JavaScript的设计方式是从我们的代码中我们不知道是怎么回事。例如，我们不能销毁对象：

```javascript
let junk = {};
junk = null; // Doesn't necessarily destroy an object
```

而且，JavaScript是一们拥有垃圾回收功能的语言。

这意味着尽管我们不能销毁一个对象，如果无法通过代码中的导线跟踪它，它最终可能消失。

![](/blog_imgs/just_javascript//05/obj1.png)

JavaScript不能保证垃圾收集何时发生。

除非你想弄清楚为什么一个应用程序使用了太多的内存，否则你不需要经常考虑垃圾收集。我在这里只提到它是为了让你知道我们可以创造对象，但我们不能销毁它们。

在我的宇宙中，对象和函数漂浮在离我的代码最近的地方。这提醒我，我可以操纵他们，甚至更多地使用它们。

## Functions

!![](/blog_imgs/just_javascript//05/fun.png)

将函数看作与代码分离的值是特别奇怪的。毕竟，它们也是我写的代码。不是吗？

### 函数也是值

我们定义函数，以便以后调用它们并在其中运行代码。然而，要真正理解JavaScript中的函数，我们需要暂时忘记它们为什么有用。相反，我们将函数看作是另一种值：一个数字、一个对象、一个函数。

为了理解函数，我们将它们与数字和对象进行比较。

首先，考虑运行console.log（2）七次的循环：

```javascript
for (let i = 0; i < 7; i++) {
  console.log(2);
}
```

**它给`console.log()`传递给多少不同的值。**为了回答这个问题，让我们回忆一下当我们写下2时是什么意思。字面上它是一个数字，文字是一种表达式——这对我们宇宙来说是个问题。在我们宇宙中，每个数字只有一个值，所以它通过每次“调用”相同的值——数字2的来“回答”我们的问题。**所以答案是一个值，**我们将看到七次打印，但每次调用都传递相同的值。

现在来简单的复习一下对象。

下面是运行`console.log({})`七次的另一个for循环：

```javascript
for (let i = 0; i < 7; i++) {
  console.log({});
}
```

**现在它传递给多少不同的值给`console.log()`？**在这里，`{}`也是一个文本——不同的是它是一个对象文本。正如我们刚刚了解到的，JavaScript宇宙不会通过调用任何东西来“回答”一个对象。相反，他会创建新的对象值——这是`{}`对象文本的结果。**所以上面的代码创建并记录了七个完全不同的对象值。**

先把它抛之脑后。

现在让我们来看看函数。

```javascript
for (let i = 0; i < 7; i++) {
  console.log(function() {});
}
```

How many different values does this code pass to console.log?
**现在它传递给多少不同的值给`console.log()`？**

![](/blog_imgs/just_javascript//03/spoilers.jpg)

在你决定答案之前不要再滚动。


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

答案是7。

**每当我们执行一行包含函数表达式的代码时，一个全新的函数值就会出现在我们的宇宙中。**

![](/blog_imgs/just_javascript//05/fun1.gif)

**这里，`function(){}`也是一个表达式**。与任何表达式一样，函数表达式向JavaScript宇宙提出一个“问题”——它通过每次我们提问时创建一个新的函数值来回答我们。这与`{}`在执行时创建新对象值的方式非常相似。函数就像对象！

严格来说，函数是JavaScript中的对象。我们将继续将它们视为单独的基本类型，因为它们与常规对象相比具有独特的功能。但是，一般来说，如果你能对一个对象做些什么，你也可以对一个函数做。它们是非常特殊的对象。

### 调用函数

下面的代码打印什么？

```javascript
let countDwarves = function() { return 7; };
let dwarves = countDwarves;
console.log(dwarves);
```

你可能认为它打印7，特别是如果你不仔细看的话。

现在检查控制台中的这个片段！它打印的确切内容取决于浏览器，但您将看到函数本身，而不是那里的数字7。

如果你遵循我们的思维模型，这种表现应该是有道理的：

1. 首先，我们用一个`function(){}`表达式创建了一个新的函数值，并将`countdwaves`变量指向这个值。
2. 接下来，我们将`dwarves`变量指向`countDwarves`所指向的值——这是相同的函数值
3. 最后，我们输出了`dwarves`当前指向的值。

在任何时候，我们都没有调用函数！

结果，`countDwarves`和`dwarves`都指向同一个值，这恰好是一个函数。所以，函数是值，我们可以将变量指向它们，就像处理数字或对象一样。

**当然，如果我们想调用函数，我们也可以这样做：**

```javascript
let countDwarves = function() { return 7; };
let dwarves = countDwarves(); // () is a function call
console.log(dwarves);
```

注意，let声明和=赋值都与函数调用无关。是`()`执行函数调用——而且是单独执行的！

添加`()`改变了代码的含义：

* 让`dwarves=countDwarves`意味着“将`dwarves`指向`countDwarves`所指向的值”

* let`dwarves=countDwarves()`表示“将`dwarves`指向`countDwarves`所指向的函数**返回的值**。”

实际上，`countDwarves()`也是一个表达式。它被称为调用表达式。为了“应答”调用表达式，JavaScript在函数内部运行代码，并将返回的值作为结果（在本例中是7）。

我们将在未来的模块中更详细地研究函数调用。

# 总结

那真是一段不同寻常的旅程！在最后两个模块中，我们看了JavaScript中的每个值类型。让我们结合计算伯爵来概括每种类型有多少个值，从不同的原始类型开始：

![](/blog_imgs/just_javascript//05/primitive-type.png)

* `Undefined`：仅仅只是个值，表示没有定义。
* `Null`：一个值，空。
* `Booleans`：两个值: true 和 false。
* `Numbers`： 数学中每个浮点数的值。
* `BigInts`：每一个可能的整数的值。
* `Strings`：每个可能的字符串的值。
* `Symbols`：我们暂时跳过了Symbols，但总有一天我们会讨论到它们的！

以下类型是特殊的，因为它们让我们可以创造自己的价值：

![](/blog_imgs/just_javascript//05/special-type.png)

* `Objects`：表示执行的每个对象文本都的值。
* `Function`：执行的每个函数表达式的值。

访问JavaScript的不同“天体”很有趣。现在我们已经计算了所有的值，我们也了解了是什么使它们彼此不同。例如，写2或“hello”总是“调用”相同的数字或字符串值。但是编写`{}`或`function()`{}`总是会创建一个全新的、不同的值。这个概念对于理解JavaScript中的相等至关重要，这将是下一个模块的主题。

# 练习

本单元也有练习题供你练习！

[点击这里，通过一些简短的练习巩固这个思维模型。](https://el2.convertkit-mail.com/c/r8up8kx6pxc9u0274lh2/mot7h6u0zplx4n/aHR0cHM6Ly9lZ2doZWFkaW8udHlwZWZvcm0uY29tL3RvL1NURWVNeT9lbWFpbD1ha2loaTk1QGdtYWlsLmNvbQ==)

**不要跳过它们！**

即使你可能熟悉不同类型的值，这些练习将帮助你巩固我们正在建立的思维模型。我们需要打好基础才能继续更复杂的话题。



































