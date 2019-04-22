# 解读 JavaScript 之引擎、运行时和堆栈调用

随着 JavaScript 变得越来越流行，很多团队在他们的堆栈中实现诸多层级的支持 - 前端、后端、混合应用程序、嵌入式设备等等。

本文是该系列文章的第一篇，旨在深入研究 JavaScript 及其实际工作原理：我们认为通过了解 JavaScript 的构建块以及它们如何一起协作的，你将能够编写更好的代码和应用。

如 [GitHut](http://githut.info/) 统计中所示，JavaScript 在 GitHub 中的活动存储库和总推送量方面位居前列。但它在其他分类中也未落后太多。

![img](https://static.oschina.net/uploads/space/2017/1213/103927_JMgU_2896879.png)

*(查看 GitHub 语言统计最新版)*

如果项目越来越依赖于 JavaScript ，这意味着开发人员必须利用语言和生态系统所提供的所有内容，深入了解其内部，从而构建出令人惊叹的软件。

事实证明，很多开发人员每天都在使用 JavaScript ，但他们并不知道底层会发生什么。

### 概述

几乎每个人都已经听说过 V8 引擎这个概念，大多数人都知道 JavaScript 是单线程的，或者它正在使用回调队列。

在这篇文章中，我们将详细介绍所有这些概念，并解释 JavaScript 是如何运行的。通过了解这些细节，你将能够编写更好的、非阻塞的应用程序，正确使用所提供的 API 。

如果你对 JavaScript 比较生疏，本博客文章将帮助你理解为什么 JavaScript 相比与其他语言更“怪异”。

如果你是一位经验丰富的 JavaScript 开发人员，希望能够为你提供一些关于你每天使用的 JavaScript 运行时的实际工作情况的全新见解。

### **JavaScript 引擎**

Google V8 引擎是一个比较流行的 JavaScript 引擎示例。V8 引擎是在诸如 Chrome 和 Node.js 等内部使用的。下面是对其机制的一个简化视图：

![img](https://static.oschina.net/uploads/space/2017/1213/104028_Z7p2_2896879.png)

该引擎包括两个主要组件：

\* Memory Heap 内存堆 ——  这是内存分配发生的地方

\* Call Stack 调用堆栈 ——  这是在你代码执行时栈帧存放的位置

### **Runtime 运行时**

几乎所有的 JavaScript 开发者都使用过浏览器中的 API（例如“setTimeout”）。 但是，这些 API 不是由引擎提供的。

那么，它们从哪里来呢？

事实证明，实际情况有点复杂。

![img](https://static.oschina.net/uploads/space/2017/1213/104047_yNc9_2896879.png)

所以，我们有引擎，但实际上还有更多。我们有那些由浏览器所提供的称为 Web API 的东西，比如 DOM、AJAX、setTimeout 等等。

然后，我们还有非常流行的**事件循环和回调队列**。

### Call Stack 调用堆栈

JavaScript 是一种单线程编程语言，这意味着它只有一个 Call Stack 。因此，它一次仅能做一件事。

Call Stack 是一个数据结构，它基本上记录了我们在程序中的所处的位置。如果我们进入一个函数，我们把它放在堆栈的顶部。如果我们从一个函数中返回，我们弹出堆栈的顶部。这是所有的堆栈可以做的东西。

我们来看一个例子。看看下面的代码：

```
function multiply(x, y) {
    return x * y;
}
function printSquare(x) {
    var s = multiply(x, x);
    console.log(s);
}
printSquare(5);
```

当引擎开始执行这个代码时，Call Stack 将会变成空的。之后，执行的步骤如下：

![img](https://static.oschina.net/uploads/space/2017/1213/104147_KJwy_2896879.png)

Call Stack 的每个入口被称为 **Stack Frame（栈帧）。**

这正是在抛出异常时如何构建 stack trace 的方法 - 这基本上是在异常发生时的 Call Stack 的状态。看看下面的代码：

```
function foo() {
    throw new Error('SessionStack will help you resolve crashes :)');
}
function bar() {
    foo();
}
function start() {
    bar();
}
start();
```

如果这是在 Chrome 中执行的（假设这个代码在一个名为 foo.js 的文件中），那么会产生下面的 stack trace：

![img](https://static.oschina.net/uploads/space/2017/1213/104225_sJsM_2896879.png)

“Blowing the stack”—当达到最大调用堆栈大小时，会发生这种情况。这可能会很容易发生，特别是如果你使用递归，而不是非常广泛地测试你的代码。看看这个示例代码：

```
function foo() {
    foo();
}
foo();
```

当引擎开始执行这个代码时，它首先调用函数“foo”。然而，这个函数是递归的，并且开始调用自己而没有任何终止条件。所以在执行的每个步骤中，同一个函数会一次又一次地添加到调用堆栈中。它看起来像这样：
![img](https://static.oschina.net/uploads/space/2017/1213/104326_4B9u_2896879.png)
然而，在某些情况下，调用堆栈中函数调用的数量超出了调用堆栈的实际大小，浏览器通过抛出一个错误（如下所示）来决定采取行动：
![img](https://static.oschina.net/uploads/space/2017/1213/104350_JCtB_2896879.png)
在单线程上运行代码可能非常容易，因为你不必处理多线程环境中出现的复杂场景，例如死锁。

但是在单线程上运行也是非常有限的。由于JavaScript只有一个调用堆栈，**所以当事情很慢时会发生什么？**

**并发&事件循环**

如果在调用堆栈中执行的函数调用需要花费大量时间才能进行处理，会发生什么？ 例如，假设你想在浏览器中使用 JavaScript 进行一些复杂的图像转换。

你可能会问 - 为什么这会是一个问题？问题是，虽然调用堆栈有要执行的函数，浏览器实际上不能做任何事情 - 它被阻塞了。这意味着浏览器无法渲染，它不能运行任何其他代码，它就是被卡住了。如果你想在你的应用程序中使用流畅的 UI ，这就会产生问题。

而且这并不是唯一的问题。一旦你的浏览器开始在 Call Stack 中处理过多的任务，它可能会停止响应相当长的时间。大多数浏览器会通过触发错误来采取行动，询问你是否要终止网页。

![img](https://static.oschina.net/uploads/space/2017/1213/104412_G1jc_2896879.png)

所以，这并不是最好的用户体验，对吗？

那么，我们如何执行大量代码而不阻塞 UI 使得浏览器无法响应？ 解决方案就是异步回调。

这将在“ JavaScript 工作原理”教程的第2部分中更详细地解释：[“V8 引擎内部+关于如何编写优化代码的5个技巧”。](https://www.oschina.net/translate/how-does-javascript-actually-work-part-2)

同时，如果你在 JavaScript 应用程序中难以复现和理解问题，请查看 SessionStack 。 SessionStack 会记录你的 Web 应用中的所有东西：所有的 DOM 更改、用户交互、JavaScript 异常、堆栈跟踪、网络请求失败、调试消息等。

通过 SessionStack ，你可以以视频的方式重现问题，并查看发生在用户身上的所有事情。

这有一个免费的方案，所以你可以[试试看](https://www.sessionstack.com/?utm_source=medium&utm_medium=blog&utm_content=Post-3-v8-getStarted)。

![img](https://static.oschina.net/uploads/space/2017/1213/111346_IexD_2896879.png)

原文地址：<https://www.oschina.net/translate/how-does-javascript-actually-work-part-1>



