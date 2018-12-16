---
title: '[翻译]理解异步 JavaScript-了解 JavaScript 如何工作'
date: 2018-12-17 01:01:36
tags: ['翻译', '前端']
---
>原文：[understanding-asynchronous-javascript-the-event-loop](https://blog.bitsrc.io/understanding-asynchronous-javascript-the-event-loop-74cd408419ff)

JavaScript 是一种单线程语言，这意味着一次只能发生一件事情。也就是说，JavaScript 引擎只能在单个线程中一次处理一个语句。

虽然单线程语言简化了代码编写，因为你无需担心并发问题，但是这也意味着您无法在不阻塞主线程的情况下执行网络访问等长时间操作。
<!--more-->

想象一下从 API 请求一些数据。根据不同的情况，服务器可能需要一些时间来处理请求，同时阻塞主线程使得网页无响应。

这就是异步 JavaScript 发挥作用的地方。使用异步 JavaScript（如回调，promise，和 async/await），你可以在不阻塞主线程的情况下执行长时间的网络请求。

虽然没有必要为了成为一名出色的 JavaScript 开发人员而将这些概念全部都学会，但是了解以下内容还是很有帮助的:）

所以不用多说了，让我们开始吧:)

## 同步 JavaScript 是如何工作的？
在深入研究异步 JavaScript 之前，让我们先了解一下同步 JavaScript 代码在 JavaScript 引擎中是如何执行的。例如：
```js
  const second = () => {
    console.log('Hello there!');
  };

  const first = () => {
    console.log('Hi there!');
    second();
    console.log('The End');
  };

  first();
```

要理解上述代码在 JavaScript 引擎中是如何执行的，我们必须要理解执行上下文和调用栈（也叫执行栈）的概念。

### 执行上下文
执行上下文是评估和执行  JavaScript 代码的环境的抽象概念。每当在 JavaScript 中运行时任何代码时，它都在执行上下文中运行。

函数代码在函数的执行上下文中执行，全局代码在全局执行上下文中执行。每个函数都有自己的执行上下文。

### 调用栈
顾名思义，调用栈是一个具有LIFO（后进先出）结构的堆栈没用于存储代码执行期间所创建的执行上下文。

因为 JavaScript 是单线程编程语言，所以 JavaScript 只有一个调用栈。这个调用栈有具有 LIFO 结构，也就意味着只能从栈顶添加或者删除项。 

让我们回到前面的代码片段并且试着来理解代码在 JavaScript 引擎中是如何执行的。
```js
  const second = () => {
    console.log('Hello there!');
  };

  const first = () => {
    console.log('Hi there!');
    second();
    console.log('The End');
  };

  first();
```
![上述代码调用栈](/images/callstack.png "上述代码调用栈")
	
### 所以发生了什么？
执行这段代码时，将创建一个全局执行上下文（由 `main()` 表示）并且将它推入了调用栈的顶部。当遇到对 `first()` 的调用时， 就将它推到栈的顶部。

接着，`console.log('Hi there!')` 被推入栈的顶部，当它执行完成时，就从栈中弹出。之后，我们调用 `second()`，因此 `second()` 函数被推入栈的顶部。

`console.log('Hello there!')`   被推到堆栈顶部并在执行完成后从堆栈中弹出。`Second()` 函数完成，因此将它从栈中弹出。

`console.log('The End')` 被推入栈顶并在执行完成后从栈中弹出。随后，`first()` 函数完成，因此它从栈中离开。

这个时候程序执行完成，因此全局执行上下文（`main()`）从栈中弹出。

## 异步 JavaScript 是如何工作的？
现在我们已经了解了调用栈的基本概念，以及同步 JavaScript 的工作原理，让我们回到异步 JavaScript。

### 什么是阻塞？ 
假设我们正在以同步的方式进行图像处理或网络请求。例如：
```js
  const processImage = (image) => {
  /**
  * 对图像进行一些操作
  **/
    console.log('Image processed');
  };

  const networkRequest = (url) => {
    /**
    * 请求网络资源
    **/
    return someData;
  };

  const greeting = () => {
    console.log('Hello World');
  };

  processImage(logo.jpg);
  networkRequest('www.somerandomurl.com');
  greeting();
```
进行图像处理和网络请求是需要时间的。因此当 `processImg()` 函数被调用时，这需要花费的时间取决于图片的尺寸。

当 `processImg()` 函数完成时，它将会从调用栈中移除。在这之后，调用 `networkRequest()` 函数并将其推入栈中。同样，它也需要花费一些时间才能完成执行。

最后当 `networkRequest()` 函数完成，`greeting()` 函数被调用，由于它只包含了一个 `console.log` 语句，而 `console.log` 语句通常很快，因此 `greeting()` 函数会立即执行并返回。

所以你看，我们必须等到函数（例如 `processImg()` 或者 `networkRequest()` ）完成。这意味着这些函数阻塞了调用栈或主线程。因此，当执行上述代码时，我们不能执行其他任何操作。这是不理想的。

### 解决方案是什么？
最简单的解决方案是异步回调。我们使用异步回调来使代码不阻塞。例如：
```js
  const networkRequest = () => {
    setTimeout(() => {
      console.log('Async Code');
    }, 2000);
  };

  console.log('Hello World');
  networkRequest();
```

这里，我使用了 `setTimeout` 方法来模拟网络请求。请记住，`setTimeout` 并不是 JavaScript 引擎的一部分，它是 web API（在浏览器中）和 C/C++ API（在 node.js 中）的一部分。

要理解这段代码是如何执行的，我们必须要理解一些其他的概念，例如事件循环和回调队列（也被称为任务队列或者消息队列）。

![JavaScript 运行时环境概述](/images/runtime.png "JavaScript 运行时环境概述")
 事件循环，web API 和消息队列/任务对列都不是 JavaScript 引擎的一部分，而是浏览器 JavaScript 运行时环境或者 Nodejs JavaScript 运行时环境（在 Nodejs 的情况下）的一部分。在 Nodejs 中，web API  被 C/C++ API 所取代。

现在，让我们回到上述代码，看看它是如何用异步的方式来执行的。
```js
  const networkRequest = () => {
    setTimeout(() => {
      console.log('Async Code');
    }, 2000);
  };

  console.log('Hello World');

  networkRequest();

  console.log('The End');
```
![事件循环](/images/eventloop.gif "事件循环")

当上述代码在浏览器中加载时，`console.log('Hello World')` 被推入调用栈并在完成之后从调用栈中弹出。接着，遇到对 `networkRequest()` 的调用，因此将它推入栈顶。

接下来调用 `setTimeout()` 函数，因此它也被推入栈顶。`setTimeout()` 有两个参数：1）回调；2）以毫秒（ms）为单位的时间。

`setTimeout()` 方法在web API 环境中启动了一个 2s 的计时器。此时 `setTimeout()` 已完成，并且从调用栈中弹出。在这之后，`console.log('The End')` 被推入调用栈，执行然后在完成之后从调用栈中弹出。

同时，计时器已过期，现在回调被推入了消息队列中。但是回调并不会立刻被执行，这就是事件循环开始的地方。

### 事件循环（Event Loop）
事件循环的作用是查看调用栈并确定调用栈是否为空。如果调用栈为空，它就会去查看消息队列，看看是否有任何挂起的回调等待执行。

在这个例子中，调用栈为空时消息队列里有一个回调。因此事件循环将回调推入栈顶。

之后，`console.log('Async Code')` 被推入栈顶，执行并从栈中弹出。此时，回调已经完成，因此将其从栈中移除，程序最终完成。

### DOM 事件
消息队列还包含来自 DOM 事件的回调，诸如点击事件和键盘事件。例如：
```js
  document.querySelector('.btn').addEventListener('click',(event) => {
    console.log('Button Clicked');
  });
```

在 DOM 事件的情况下，事件监听器位于 web API 环境中，等待着某个事件（例子中是点击事件）发生，当事件发生时，回调函被放入消息队列中，等待被执行。

事件循环会再一次检查调用栈是否为空，如果为空就将事件回调推送到调用栈中并且执行回调。

我们已经学了异步回调和   事件是如何执行的，它们使用消息队列来存储所有等待执行的回调。

### ES6 Job Queue(作业队列)/Micro-Task（微任务队列）
ES6 中引入了 Promise 在 JavaScript 中的使用的作业队列/微任务队列的概念。消息队列和作业队列的区别在于作业队列的优先级比消息队列有更高，这意味着在作业队列/微任务队列中的 promise 任务将会比在消息队列中的回调先执行。

例如：
```js
  console.log('Script start');

  setTimeout(() => {
    console.log('setTimeout');
  }, 0);

  new Promise((resolve, reject) => {
      resolve('Promise resolved');
    }).then(res => console.log(res))
      .catch(err => console.log(err));

  console.log('Script End');
```
输出：
```js
  Script start
  Script End
  Promise resolved
  setTimeout
```
我们可以看到，promise 在 `setTimeout` 之前执行，因为 promise 的响应存储在微任务队列中，其优先级高于消息队列。

让我们再看另一个例子，这次有两个 promise 和两个 setTimeout。
例如：
```js
  console.log('Script start');
  setTimeout(() => {
    console.log('setTimeout 1');
  }, 0);

  setTimeout(() => {
    console.log('setTimeout 2');
  }, 0);

  new Promise((resolve, reject) => {
      resolve('Promise 1 resolved');
    }).then(res => console.log(res))
      .catch(err => console.log(err));

  new Promise((resolve, reject) => {
      resolve('Promise 2 resolved');
    }).then(res => console.log(res))
      .catch(err => console.log(err));

  console.log('Script End');
```
打印输出：
```js
  Script start
  Script End
  Promise 1 resolved
  Promise 2 resolved
  setTimeout 1
  setTimeout 2
```

我们可以看到两个 promise 在 `setTimeout` 的回调之前执行，因为事件循环将微任务中的任务的优先于消息队列中的任务。

当事件循环正在执行处于微任务队列中的任务时，如果另一个 promise 决议了，它将会被添加到同一个微任务队列的末尾，并且它将会比在消息队列中的回调先执行，无论回调等待执行的时间有多长。

例如：
```js
  console.log('Script start');

  setTimeout(() => {
    console.log('setTimeout');
  }, 0);

  new Promise((resolve, reject) => {
      resolve('Promise 1 resolved');
    }).then(res => console.log(res));

  new Promise((resolve, reject) => {
    resolve('Promise 2 resolved');
    }).then(res => {
        console.log(res);
        return new Promise((resolve, reject) => {
          resolve('Promise 3 resolved');
        })
      }).then(res => console.log(res));

  console.log('Script End');
```
打印输出：
```js
  Script start
  Script End
  Promise 1 resolved
  Promise 2 resolved
  Promise 3 resolved
  setTimeout
```
因此，微任务队列中的所有的任务都会比消息队列中的任务先执行。也就是说，事件循环会先将微任务队列中清空再执行位于消息队列中的回调。

## 总结
因此，我们已经了解了异步 JavaScript 如何工作以及其他的概念，例如调用栈，事件循环，消息队列/任务队列和作业队列/微任务队列，它们共同构成了 JavaScript 运行时环境。虽然没有必要为了成为一名出色的 JavaScript 开发人员而学习所有这些概念，但是了解这些概念是有帮助的。

就是这样，如果你发现这篇文章有用，请点击拍手👏按钮，你也可以在 [Medium](https://medium.com/@Sukhjinder) 和 [Twitter](https://twitter.com/sukhjinder_95) 上关注我，如果你有任何疑问，请随时发表评论！ 我很乐意帮忙:)






