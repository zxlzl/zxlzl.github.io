---
title: 《Just-JavaScript》08-突变
date: 2020-07-15 11:36:26
tags:
---


在上一个关于属性的模块中，我们介绍了福尔摩斯搬到马里布的奥秘。但我们还没有对它进行解释。

打开一个[素描应用程序](https://excalidraw.com/?ck_subscriber_id=767004595)或拿一支笔和一张纸。**这一次，我们将一步一步地绘制示意图**，这样你就可以检查你的思维模型了。

虽然你早前自己试过了，但多练习也无妨！在本单元的最后，我们将讨论这个例子背后的更多的知识。

## 第1步：声明sherlock变量

我们从这个变量声明开始：

```js
let sherlock = {
  surname: 'Holmes',
  address: { city: 'London' }
};
```

现在开始绘制示意图的步骤。

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你画出示意图之前不要再滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

你的图表应该是这样的：

![](/blog_imgs/just_javascript/08/sherlock.png)

有一个sherlock变量指向一个对象。该对象有两个属性。它的`surname`属性指向“Holmes”字符串值。它的`address`属性指向另一个对象。另一个对象只有一个名为`city`的属性。该属性指向“London”字符串值。

仔细看看我绘制这个图表的过程：

![](/blog_imgs/just_javascript/08/sherlock.gif)

你的过程相似吗？

### 无嵌套对象

请注意，这里不是一个，而是两个完全独立的对象。两对大括号意味着两个对象。

**对象可能在代码中显示为“嵌套”，但在我们的宇宙中，每个对象都是完全独立的。一个对象不能在其他对象的“内部”！**

如果你仍然认为对象是嵌套的，现在就试着摆脱这个想法。

## 第2步：声明john变量

在此步骤中，我们声明另一个变量：

```js
let john = {
  surname: 'Watson',
  address: sherlock.address
};
```

编辑之前绘制的图表以反映这些更改。

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你画出示意图之前不要再滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

你在图表中添加的内容应如下所示：

![](/blog_imgs/just_javascript/08/john.png)

现在还有一个`john`变量。它指向具有两个属性的对象。它的`address`属性指向`sherlock`的`address`属性已经指向的地方。它的`surname`属性指向“Watson”字符串。

可以详细了解我的流程：

![](/blog_imgs/just_javascript/08/john.gif)

你做了什么不同的事吗？

### 属性总是指向值

当您看到`address`时：`sherlock.address`，我们很容易认为`John`的`address`属性指向`Sherlock`的`address`属性。

这是误导。

**记住：属性总是指向一个值！它不能指向另一个属性或变量。一般来说，宇宙中所有的导线都指向值。**

![](/blog_imgs/just_javascript/08/point.png)

当我们看到`address`时：`sherlock.address`，我们必须计算出`sherlock.address`，并将地址属性线指向该值。重要的是值本身，而不是我们如何找到它`(sherlock.address)`。

因此，现在有两个不同的对象，它们的`address`属性指向同一个对象。你能在图表上找到它们吗？

## 第3步：更改属性

现在让我们回顾一下属性模块中示例的最后一步。

`John`身陷身份危机，厌倦了伦敦的细雨。他决定改名，搬到马里布。我们通过设置一些属性做了这件事：

```js
john.surname = 'Lennon';
john.address.city = 'Malibu';
```

我们如何通过改变图表来反映它？

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你画出示意图之前不要再滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

你的图表应该是这样的：

![](/blog_imgs/just_javascript/08/leave.png)

`john`变量指向的对象现在有一个指向“Lennon”字符串值的`name`属性。更有趣的是，`john`和`sherlock`的`address`属性指向的对象现在具有不同的`city`属性值。它现在指向“Malibu”字符串。

在一起奇怪的地点劫持案中，夏洛克和约翰最终都在马里布。按照图中的接线图进行操作，并验证是否正确。

```js
console.log(sherlock.surname); // "Holmes"
console.log(sherlock.address.city); // "Malibu"
console.log(john.surname); // "Lennon"
console.log(john.address.city); // "Malibu"
```

以下是我最后一系列更改的过程：

![](/blog_imgs/just_javascript/08/leave.gif)

我们弄清连线，然后是值，最后把导线指向那个值。

这个结果现在应该讲得通了，但是这个例子在更深层次上令人困惑。哪里出错了？我们如何真正修复代码，让约翰独自一人搬到马里布？为了弄清楚，我们需要谈谈突变。

# 突变

突变是“改变”一种别致的说法。

**例如，我们可以说我们改变了一个对象的属性，或者我们可以说我们改变了这个对象（及其属性）。这是同一件事。**

人们喜欢说“突变”，因为这个词有一种阴险的基调。它提醒你要格外小心。这并不意味着突变是“坏的”——这只是编程而已！-但你需要非常有意识地去做这件事。

让我们回忆一下我们最初的任务。我们想给约翰换个姓，把他搬到马里布。现在让我们看看我们的两个突变：

```js
// Step 3: Changing the Properties
john.surname = 'Lennon';
john.address.city = 'Malibu';
```

哪些对象正在发生突变？

第一行改变了`john`指向的对象——具体地说，是它的`surname`属性。这是可以理解的：事实上，我们的意思是改变`john`的`surname`。那个对象代表John的数据。所以我们改变了它的姓氏属性。

然而，第二行却有很大的不同。它不会改变`john`指向的对象。相反，它改变了一个完全不同的对象——我们可以通过`john.address`到达. 如果我们看这个图，我们知道它是同一个对象，我们也可以通过`sherlock.address`到达!

**通过改变程序中其他地方使用的对象，我们把事情搞得一团糟。**

## 可能的解决方案：改变另一个对象

解决此问题的一种方法是避免更改共享数据：

```js
// Replace Step 3 with this code:
john.surname = 'Lennon';
john.address = { city: 'Malibu' };
```

第二行的区别是微妙的，但非常重要。

当我们得到`john.address.city = "Malibu"`，左边的导线是`john.address.city`. 我们通过`john.address`改变了这个这个对象的`city`属性。但同样的对象也可以通过`sherlock.address`得到。结果，我们无意中改变了共享数据。

通过`john.address = { city: 'Malibu' }`，导线的左边是`john.address`，我们正在改变`john`指向的对象的`address`属性。换句话说，我们只是改变了代表`John`数据的对象。这就是`sherlock.address.city`保持不变的原因：


![](/blog_imgs/just_javascript/08/unchanged.png)

如你所见，视觉上相似的代码可能会产生非常不同的结果。一定要注意哪根导线在赋值的左边！

## 替代方案：不要改变对象

我们还有另一种方法让`john.address.city`为`"Malibu"`,而`sherlock.address.cit`仍然是`"London"`：

```js
// Replace Step 3 with this code:
john = {
  surname: 'Lennon',
  address: { city: 'Malibu' }
};
```

在这里，我们根本没有改变`John`的对象。相反，我们重新指定`john`变量以指向`john`数据的“新版本”。从现在起，`john`指向另一个对象，该对象的地址也指向一个全新的对象：

![](/blog_imgs/just_javascript/08/reassign.png)


你可能会注意到，现在在我们的图表中有一个“废弃的”旧版本的`John`对象。我们不用担心。如果没有导线指向它，JavaScript最终会自动将其从内存中删除。

请注意，这两种方法都满足我们的所有要求：

```js
console.log(sherlock.surname); // "Sherlock"
console.log(sherlock.address.city); // "London"
console.log(john.surname); // "Lennon"
console.log(john.address.city); // "Malibu"
```

比较他们的示意图。你对这两种方法有个人偏好吗？你认为他们的优点和缺点是什么？

## 向夏洛克学习

福尔摩斯曾经说过：“当你排除了不可能，剩下的，无论多么不可能，都必须是真相。”

**当你的思维模型变得更完整时，你会发现调试问题更容易，因为你会知道要寻找什么可能的原因。**

例如，如果你知道`sherlock.address.city`在运行一些代码后发生了变化，图中的连线给出了三种解释：

![](/blog_imgs/just_javascript/08/explanations.png)

1. 也许`sherlock`变量被重新分配了。
2. 也许我们通过`sherlock`可以到达的对象发生了变化，它的`address`属性被设置为不同的东西。
3. 也许我们通过`sherlock.address`可以到达的对象变化了，它的属性`city`被设置成不同的值。

你的思维模型为你提供了一个起点，你可以从中研究bug。这也正好相反。有时候，你可以看出一段代码不是问题的根源，因为思维模型证明了这一点！

假设，如果我们将`john`变量指向另一个对象，我们可以相当肯定`sherlock.address.city`不会改变的。我们的图表显示，改变`john`导线不会影响任何从`sherlock`开始的链条：

![](/blog_imgs/just_javascript/08/affect.png)

不过，请记住，除非你是福尔摩斯，否则你很难对某些事情充满信心。这种方法和你的思维模式一样好！思维模型可以帮助你提出理论，但你需要设计实验，这样你才能用通过控制台或者调试器来证实它们。

## Let vs Const

值得注意的是，您可以使用const关键字作为替代：

```js
const shrek = { species: 'ogre' };
```

const关键字允许你创建只读变量——也称为常量。一旦我们声明了一个常量，就不能将它指向另一个值：

```js
shrek = fiona; // TypeError
```

但有一点很关键。**我们仍然可以改变object const指向：**

```js
shrek.species = 'human';
console.log(shrek.species); // 'human'
```

![](/blog_imgs/just_javascript/08/const.png)

在这个例子中，只有`shrek`变量线本身是只读的（`const`）。它指向一个对象——并且该对象的属性可以被改变！

`const`的有用性是一个热议的话题。有些人喜欢完全禁止`let`，并且总是使用`const`。其他人可能会说，应该信任程序员重新分配他们自己的变量。无论你的偏好是什么，请记住`const`防止变量重新分配，而不是对象改变。

## 突变有害吗？

我想确保你不会轻易获得这个想法——突变是“坏的”。这将是一个懒惰的过度简化，模糊了真正的理解。如果随着时间的推移，某个数据发生了改变。问题是什么会发生改变，在哪里，什么时候。这也是一个备受争议的话题。

突变是“远距离的恐怖行为”。改变`john.address.city`导致`console.log(sherlock.address.city)`打印其他东西。

**当你改变一个对象时，变量和属性可能已经指向它了。你的改变会影响以后“跟随”这些连线的任何代码。**

这既是福也是祸。变异使更改某些数据变得很容易，并立即“看到”整个程序中的更改。然而，不受约束的突变使得预测程序会做什么变得更加困难。

有一个学派认为，最好将突变控制在应用程序的一个非常窄的层中。缺点是你可能会编写更多的样板代码来“传递信息”。但是好处是根据这一理念，程序的行为将变得更加可预测。

值得注意的是，对刚创建的对象进行变异总是可以的，因为还没有其他导线指向它们。在其他情况下，我建议你对你正在发生的改变以及何时发生改变要非常小心。你对改变的依赖程度取决于你的应用程序的架构。

# 总结

- 对象永远不会“嵌套”在我们的宇宙中。
- 密切注意赋值左侧的导线。
- 改变一个对象的属性也叫做改变这个对象。
- 如果你改变了一个对象，你可以“看到”这个变化通过指向该对象的任何导线。有时候，这可能是你想要的。但是，意外更改共享数据可能会导致错误。
- 改变你刚刚在代码中创建的对象是安全的。大体上，你会使用多少改变取决于你的应用程序的架构。即使你不会经常使用它，也值得你花时间去了解它的工作原理。
- 可以用`const`而不是`let`声明变量。这允许您强制此变量的连线始终指向同一值。但请记住，const不能阻止对象突变！

# 练习

本单元也有练习题供你练习！

[点击这里用一些简短的练习巩固这个心智模型。](https://eggheadio.typeform.com/to/Ql4IPM?email=akihi95@gmail.com&ck_subscriber_id=767004595)

**不要跳过它们！**

尽管你可能对突变的概念很熟悉，但这些练习将帮助你巩固我们正在建立的心理模型。我们需要这个基础才能得到更复杂的话题。







