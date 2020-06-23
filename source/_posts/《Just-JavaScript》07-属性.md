---
title: 《Just-JavaScript》07-属性
date: 2020-06-23 13:52:59
tags:
---

认识来自伦敦的世界著名侦探夏洛克·福尔摩斯：

```javascript
let sherlock = {
  surname: 'Holmes',
  address: { city: 'London' } 
};
```

他的朋友约翰·沃森最近搬来和夏洛克住在一起：

```javascript
let john = {
  surname: 'Watson',
  address: sherlock.address
};
```

夏洛克是个出色的侦探，但他是个难相处的室友。有一天，约翰觉得他受够了。约翰改姓，搬到马里布居住：

```js
john.surname = 'Lennon';
john.address.city = 'Malibu';
```

是时候做个小练习了。**写下你对这些问题的答案：**

```js
console.log(sherlock.surname); // ? 
console.log(sherlock.address.city); // ? 
console.log(john.surname); // ? 
console.log(john.address.city); // ? 
```

在你向上滚动以重新阅读代码之前，我希望您以特定的方式处理它。打开一个[素描应用程序](https://excalidraw.com/)，或者拿纸和笔，**画出每一行在你的思维模型上发生的事情的**。如果你不知道怎么表达也没关系。我们还没有讨论这些话题，所以尽你所能。

然后，在你的最终草图的帮助下，回答上面的四个问题。


![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你写完这四个问题的答案之前不要往下滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

现在来检查你的答案：

```js
console.log(sherlock.surname); // "Holmes"
console.log(sherlock.address.city); // "Malibu"
console.log(john.surname); // "Lennon"
console.log(john.address.city); // "Malibu"
```

这不是打字错误——他们确实都在马里布。摆脱夏洛克可不容易！如果建立了错误的思维模型，大家可能会得出这样的结论`sherlock.address.city`是`London`——但不是的。

要了解原因，我们需要了解属性在JavaScript中是如何工作的。

# 属性

我们以前讨论过对象。例如，这里有一个`sherlock`变量，它指向一个对象值。我们通过写入`{}`来创建新的对象值：

```js
let sherlock = {};
```

在我们的宇宙中，可能是这样的：

![](/blog_imgs/just_javascript/07/sherlock.png)

但是，对象是用于将相关数据分组在一起。例如，我们可能想将关于夏洛克的不同的东西进行分组：

```js
let sherlock = {
  surname: 'Holmes',
  age: 64,
};
```

在这里，`sherlock`仍然是一个变量，但`surname`和`age`不是。它们是属性。与变量不同，属性属于特定对象。

**在我们的JavaScript宇宙中，变量和属性都像“连线”一样**。但是，属性的连接从对象开始，而不是从我们的代码开始：

![](/blog_imgs/just_javascript/07/wires.png)

在这里，我们可以看到`sherlock`**变量**指向我们创建的对象。那个对象有两个**属性**。其`surname`属性指向`“Holmes”`字符串值，其`age`属性指向`64`这个数字值。

重要的是，属性不包含值——属性指向值！原来我们的宇宙充满了导线。其中一些从我们的代码（变量）开始，另一些从对象（属性）开始。所有导线始终指向值。

在阅读本文之前，您可能已经想象到值存在于对象的“内部”，因为它们在代码中显示为“内部”。这种直觉常常导致错误，因此我们将“在导线中思考”取而代之。再看一眼代码和图表。在你继续之前，确保这样思考是让你感到放松的。

## 读取属性

我们可以使用“点符号”读取属性的当前值：

```js
console.log(sherlock.age); // 64
```

在这里，`sherlock`是我们的老朋友——一个表达式，一个向JavaScript宇宙提出的问题。要回答这个问题，JavaScript首先要跟随`sherlock`导线：

![](/blog_imgs/just_javascript/07/follow.gif)

它通向一个对象。从这个对象中，JavaScript沿着导线找到`age`属性。

我们这个对象的年龄属性的值是64，所以`sherlock.age`结果是64。

## 属性名称

属性有名称。一个对象不能有两个同名的属性。例如，我们的对象不能有两个名为age的属性。

属性名称始终区分大小写！例如，从JavaScript的角度来看，age和Age是两个完全不同的属性。

如果我们事先不知道属性名，但在代码中将其作为字符串值，则可以使用`[]`“括号表示法”从对象中读取它：

```js
let sherlock = { surname: 'Holmes', age: 64 };
let propertyName = prompt('What do you want to know?');
alert(sherlock[propertyName]); // 按属性名称读取属性
```

在浏览器控制台中尝试此代码，并在提示时输入age。

## 分配给属性

当我们给属性赋值时会发生什么？

```js
sherlock.age = 65;
```

让我们用=将这段代码分为左侧和右侧。

首先，我们找出哪根导线在左边：`sherlock.age`。

我们沿着`sherlock`线，然后选择`age`属性线：

![](/blog_imgs/just_javascript/07/property.gif)

请注意，我们没有按照`age`导线到64。我们不在乎它现在的值是多少。在赋值的左边，我们正在寻找**导线本身**。

还记得我们选哪根电线吗？接下来继续。

**接下来，我们得到哪个值在右边：65。**

与左边不同，赋值的右边总是表示一个值。在本例中，右边的值是数值65。让我们召唤它：

![](/blog_imgs/just_javascript/07/summon.gif)

现在我们准备好执行任务了。

**最后，我们将左侧的导线指向右侧的值：**

![](/blog_imgs/just_javascript/07/assign.gif)

我们完成了！从现在开始，阅读`sherlock.age`会给我们65。

## 找不到的属性

你可能想知道如果我们读取不存在的属性会发生什么：

```js
let sherlock = { surname: 'Holmes', age: 64 };
console.log(sherlock.boat); // ?
```

我们知道`sherlock.boat`是一个属性表达式。JavaScript宇宙遵循一定的规则来决定用哪个值来“回答”我们的问题。

**这些规则大致如下：**

1. 在点（.）之前计算出这部分的值。
2. 如果该值为空或未定义，则立即抛出错误。
3. 检查对象中是否存在具有该名称的属性。  
    a. 如果**存在**，则使用此属性指向的值进行应答。  
    b. 如果**不存在**，用未定义的值回答。

注意这些规则有点简化，我们需要在以后的模块中对它们进行修改。不过，他们已经告诉了我们很多关于JavaScript的工作原理！

例如，sherlock指向一个**没有**boat属性的对象。所以`sherlock.boat`给我们一个未定义的答案：

```js
let sherlock = { surname: 'Holmes', age: 64 };
console.log(sherlock.boat); // undefined
```

注意这**并不意味着**我们的对象拥有指向值为`undefined`的`boat`属性！它只有两个属性，它们都不叫`boat`：

![](/blog_imgs/just_javascript/07/properties.png)

很容易思考`sherlock.boat`直接对应于我们思维模型中的属性概念，但这**并不完全正确**。这是JavaScript引擎的一个问题，因此引擎遵循规则来回答它。

它查看sherlock指向的对象，发现它**没有**boat属性，并返回undefined值，因为**规则就是这么说的**。没有更深层次的原因：计算机遵循规则。

向上滚动并重新阅读规则。你能在实践中应用它们吗？

```js
let sherlock = { surname: 'Holmes', age: 64 };
console.log(sherlock.boat.name); // ?
```

如果我们运行这个代码会怎么样？**别猜——遵守规则**。

提示：这里有两个点，所以你需要遵循规则两次。

![](/blog_imgs/just_javascript/03/spoilers.jpg)

在你得到答案之前不要再滚动。

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

`... ` 

答案是计算`sherlock.boat.name`会**引发错误**：

- 我们首先要弄清楚`sherlock.boat`。
    - 要做到这一点，我们需要找出`sherlock`的值。
        - `sherlock`变量的导线指向一个对象。
        - 因此，`sherlock`的值就是这个对象。
    - 对象不是空或者未定义，所以我们继续往下。
    - 该对象**并没有**属性`boat`。
    - 因此，`sherlock.boat`的值是undefined。
- 我们在点（.）的左边得到值undefined。
- 规则告诉我们左边为空或未定义是错误的。

```js
let sherlock = { surname: 'Holmes', age: 64 };
console.log(sherlock.boat); // undefined
console.log(sherlock.boat.name); // TypeError!
```

如果这看起来仍然令人困惑，向上滚动并机械地遵循规则。

# 总结

- 属性是导线——有点像变量。它们都指向值。与变量不同，属性**从我们宇宙中的对象开始**。
- 属性有名称。属性属于特定对象。一个对象上不能有多个同名属性。
- 通常，您可以通过三个步骤执行赋值：
  1. 找出哪根导线在左边。
  2. 找出右边的值。
  3. 把导线指向那个值。
- 像这样的表达式`obj.property`分三步计算：
  1. 找出哪个值在左边。
  2. 如果为空或未定义，则抛出错误。
  3. 如果该属性存在，则结果是其导线指向的值。
  4. 如果该属性不存在，则结果未定义。

注意，这个属性的思维模型仍然有点简化。对于接下来的几个模块来说，它已经足够好了，但是我们需要在将来扩展它。

如果你在一开始就被福尔摩斯的例子弄糊涂了，你可以有选择地回到这里，试着用我们的新思维模型来思考它。下一个模块将包括一个详细的演练，以防你仍然不太确定它为什么这样工作。试着习惯于将属性视为导线。

# 练习

本单元也有练习题供你练习！

[点击这里，通过一些简短的练习巩固这个思维模型。](https://eggheadio.typeform.com/to/IDJBPQ?email=akihi95@gmail.com&ck_subscriber_id=767004595)

**不要跳过它们！**

尽管你可能对属性的概念很熟悉，这些练习将帮助你巩固我们正在建立的思维模型。我们需要打好基础才能继续更复杂的话题。










































