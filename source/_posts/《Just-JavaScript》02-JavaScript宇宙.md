---
title: 《Just JavaScript》02.JavaScript宇宙
date: 2020-04-01 09:02:26
tags:
---

在JavaScript中，它的开始就是值。

什么是指？这很难解释。

这就好比问数学中的数字是什么，几何中的点是什么。值就是JavaScript中的类似这样的一个东西。

数字是值，但其他东西也是值，比如对象和函数。但是很多东西，比如if语句或变量声明都不是值。

## 值和代码

为了将我们的JavaScript程序中的所有值与其他值区别开来，我想象一下安东尼画的小王子：

![](/blog_imgs/just_javascript/02/little_prince.jpg)

我站在一颗小行星上——这是我程序的代码。

从表面上看，我看到了if语句和变量声明、逗号、大括号以及可能从JavaScript代码中找到的所有其他东西。

我得代码包括“函数调用”或是“多次执行此操作”甚至“抛出错误”等指令。我一步一步地完成这些指令——在我的小行星上做着差事。

但偶尔我会抬头看看。

在一个晴朗的夜晚，我在JavaScript天空中到了不同的值：booleans、numbers、strings、symbols、functions、objects、null和undefined——天呐！我可以在代码中使用他们，但他们并不存在于代码中。

在我的JavaScript宇宙中，值飘荡在太空。

![](/blog_imgs/just_javascript/02/universe.png)

“等等，”你可能会说，“我一直认为值存在于我的代码里！”。在这里，我请求你的认知发生飞跃性的变化。要想成功构建这样的思维模型，还需要几个模块。[Give It Five Minutes](https://www.jianshu.com/p/9adb15be9ac2)

回到值上面。大体上，这有两种值。

### 原始值

**原始值**包括数字和字符串等。打开浏览器的控制台并使用 `console.log()` 打印以下原始值：

``` javascript
console.log(2)
console.log("hello")
console.log(undefined)
```

所有的原始值都有一些共同点。**我的代码中没有什么可以影响他们**。这听起来有点模糊，所以我们将在下一个模块中具体探讨这是什么意思。现在，我要说的是，原始值就像星星一样——冰冷而遥远，但在我需要它们的时候总是在那里。

这是第一种值。

### 对象和函数

**对象和函数**也是值，但他们不是原始值。这使他们变得特别。继续在控制台打印一些这样的值：

``` javascript
console.log({})
console.log([])
console.log(x => x * 2)
```

请注意，浏览器的控制台是如何以不同于原始值的方式显示它们的。有些浏览器可能会在他们前面显示一个箭头，或者单点击它们的时候执行一些特殊的操作。如果你安装了一些不同的浏览器（例如Chrome和Firefox），比较它们是如何可视化对象和函数的。

对象和函数是特殊的，因为我可以从代码中操作它们。举个例子，我可以将它们与其他值连接起来。这也是模糊的——所以我们将在后面的模块中完善这个想法。现在，我可以说，
如果原始值像遥远的恒星，那么对象和函数更像漂浮在代码附近的岩石。它们离得很近，我才能够操作它们。

这就是第二种值。

你可能有问题。很好。如果你问一个问题，JavaScript世界可能会回答它！当然前提是你知道怎么提问。

## 表达式

但也有很多问题JavaScript无法回答。如果你想知道是向你最好的朋友坦白你的真实感受，还是一直等到你俩变成骷髅，JavaScript不会有多大帮助。

但是JavaScript很乐意回答这样的一些问题。这些问题有一个特殊的名字——表达式。

如果我们“询问”表达式2+2，JavaScript将用4“回答”。

``` javascript
console.log(2 + 2); // 4
```

**表达式是JavaScript可以回答的问题。JavaScript使用它唯一知道的方式——值，回答表达式。**

![](/blog_imgs/just_javascript/02/expression.gif)

如果“表达式”这个词让你感到困惑，请将其视为一段表示值的代码。你可能会听到人们说2+2“结果”或“得到”4。这些都是说同一件事的不同方式。

我们问JavaScript 2+2，它的答案是4。表达式总是得到一个值。现在我们对表达方式的了解已经足够危险了！

我之前说JavaScript有多种值：numbers、strings、objects等等。我们如何知道每次说的是哪种值呢？

这听起来是个问题。我们敢问吗？

### 检查类型

首先，JavaScript宇宙中的所有值可能看起来都一样——就像天空中的亮点。但是如果你仔细观察，你会发现只有不到十种不同类型的值。相同类型的值的行为方式类似。

如果我们想检查一个值的类型，我们可以用typeof运算符。JavaScript将用一个预先确定的字符串值来回答我们的问题，比如“number”、“string”或“object”。

![](/blog_imgs/just_javascript/02/telescope.png)

下面是一些您可以在浏览器控制台中尝试的示例:

``` javascript
console.log(typeof(2)); // "number"
console.log(typeof("hello")); // "string"
console.log(typeof(undefined)); // "undefined"
```

这里， `typeof(2)` 是一个表达式，它得到“number”值。

严格的说，typeof不需要使用括号。例如， ` typeof 2` 和 `typeof(2)` 的工作原理是相同的。然而，有时需要括号来避免歧义。如果我们在typeof后面省略了括号，下面的一个例子就会中断。试着猜猜是哪一个：

``` javascript
console.log(typeof({})); // "object"
console.log(typeof([])); // "object"
console.log(typeof(x => x * 2)); // "function"
```

你可以在浏览器控制台中验证你的猜测。

![](/blog_imgs/just_javascript/02/typeof.gif)

现在再来看看最后三个例子——这次要密切关注它们的结果。你有没有发现这些结果令人惊讶？为什么？

## 值的类型

作为一个有抱负的天文学家，您可能想知道JavaScript宇宙中可以观察到的每一种类型的值。经过将近25年的JavaScript研究，科学家们只发现了9种类型。

### 原始值

* **Undefined**(undefined)，用于无意中丢失的值
* **Null**(null), 用于故意丢失的值
* **Booleans**(true or false)，用于逻辑操作
* **Numbers**(-100, 3.14... )，用于数学计算
* **Strings**("hello", "abracadabra"... )，用于文本
* **Symbols**(不常见)，用于隐藏实现的细节
* **BigInts**(不常见、新的)，用于计算大数

### 对象和函数

* **Objects**({}... )，用于分组相关的数据和代码
* **Functions**( `x => x * 2` ... )，用于引用代码

### 没有别的类型了

你可能会问：“那我使用的别的类型呢？比如数组？”

**在JavaScript中，除了我们刚刚列举的值类型之外，没有其他基本值类型了**。剩下的都是对象！例如，甚至数组、日期和正则表达式基本上都是JavaScript中的对象：

``` javascript
console.log(typeof([])); // "object"
console.log(typeof(new Date())); // "object"
console.log(typeof(/(hello|goodbye)/)); // "object"
```

“我知道，”你可能会说，“这是因为一切都是对象！”。唉，这是一个流行的都市传说，但事实并非如此。尽管像 `"hi".toUpperCase()` 这样的代码使 `"hi"` 看起来像一个对象，但这只是一个幻觉。JavaScript会在执行此操作时创建包装器对象，然后立即将其丢弃。

如果这个机制不太好明白也没事。**现在，你只需要记住原始值（如数字和字符串）不是对象。**

## 总结

让我们回顾一下我们目前所知道的：

1. **除了值就是别的**：我们可以将值视为JavaScript雨中中“飘荡”的不同事物。它们不存在于我们的代码中，但我们可以从代码中引用它们。
2. **有两种值**：它们是原始值，然后是对象和函数。总共有九种不同类型的值，每种类型都有特定的用途，但有些很少使用。
3. **有些值很孤单**：比如null是Null类型的唯一的值，undefined也是Undefined类型的唯一值。我们之后会学习它，这两个孤独的值在很大程度上就是麻烦制造者。
4. **我们可以使用表达式提问**：JavaScript将会用值来回答我们。例如，表达式2+2的答案是4。

5.**我们可以通过typeof表达式来检测值的类型**：比如， `typeof(4)` 得到字符串“number”。

## 练习

是时候学以致用了。

即使你已经有了相当丰富的JavaScript经验，也不要跳过练习题！就在几年前，我从其中学到了一些东西。

[点击此处去做练习！](https://eggheadio.typeform.com/to/PLyTKB?email=akihi95@gmail.com&ck_subscriber_id=767004595)

接下来我们将更详细地探讨原始值。我们看看这些不同的类型（比如数字和Null）有什么共同点，并学习相等在JavaScript中意味着什么。

我们还将继续完善我们的思维模型。这部分提供了一个粗略的草图——一个近似值。我们将把焦点放在图片的不同部分，并用更多的细节填充它们，如[渐进式JPEG图像](https://www.liquidweb.com/kb/what-is-a-progressive-jpeg/?ck_subscriber_id=767004595)。

这些看起来是很小的一步，但我们正在为未来的一切奠定基础。我们正在一起构建JavaScript宇宙。

## 知识扩展

1.[The history of “typeof null”](https://2ality.com/2013/10/typeof-null.html)

