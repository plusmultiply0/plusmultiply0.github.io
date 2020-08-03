---
title: >-
  translation:javascript-tips-child-constructors-text-selection-inline-workers-and-more
tags:
  - JavaScript
  - tips
  - attempt
  - web
comments: true
category:
  - - skills
  - - notes
  - - translation
date: 2020-07-24 12:00:50
update: 2020-07-24 12:00:50
---

# 翻译：JavaScript 技巧 —— 子代构造函数，文本选择，内联 Workers 等等

**索引**：
- [取消双击后的文本选中](#preventTextSelect)
- [为什么设置原型的构造函数是必要的？](#PrototypeConstructor)
- [无需单独的 Javascript 文件即可创建 Web Workers](#WebWorkers)
- [如何去除字符串内的文件扩展名](#trimfname)
- [为什么在 Array.prototype.map 里使用 parseInt 会返回 NaN？](#mapparse)
- [使用三目运算符时省略第二个表达式](#omit)
- [清除 Yarn 中的缓存](#yarn)
- [innerText 在 IE 中有效，但在其他浏览器中无效](#innerText)
- [总结](#conclusion)

**正文：**

当我们在编写 JavaScript 应用的时候，和编写其他类型的应用程序一样，需要解决一些困难的问题。

本篇文章中，我们将会介绍常见 JavaScript 问题的一些解决方法。

## <a id="preventTextSelect"></a>取消双击后的文本选中

我们可以通过调用 <code>preventDefault()</code> 来阻止在双击后对文本的选中这个默认行为。

例如，我们可以这样写：

```javascript
document.addEventListener('mousedown', (event) => {
  if (event.detail > 1) {
    event.preventDefault();
    // ...
  }
}, false);
```

## <a id="PrototypeConstructor"></a>为什么设置原型的构造函数（Prototype Constructor）是必要的？

我们必须设置原型的构造函数 ，这样，我们就可以用 `instanceof` 检查原型的构造函数是否为一个给定的构造函数。

比如：

```javascript
function Person(name) {
  this.name = name;
}  


function Student(name) {  
  Person.call(this, name);
}

Student.prototype = Object.create(Person.prototype);
```

如此，`Student` 的原型会被设置为 `Person` 。

虽然 `Student` 继承自 `Person`，但我们实际上想要将 `Student` 的原型设置为 `Student`。

因此，我们需要这么写：

```javascript
Student.prototype.constructor = Student;
```

现在我们创建了 `Student` 的一个实例，用 `instanceof` 来检查一下实例：

```javascript
student instanceof Student
```

结果会返回 `true` 。

如果我们使用 `class` 语法，我们不必这样做了。

我们只需要写：

```javascript
class Student extends Person {
   //...
}
```

这样，所有的事情都自动为我们处理好了。

## <a id="WebWorkers"></a>无需单独的 Javascript 文件即可创建 Web Workers

使用 `javascript/worker` 作为 `type` 属性的值，可以让我们无需单独创建 JavaScript 文件就能创建一个 web worker。

例如，我们可以这样写：

```html
<script id="worker" type="javascript/worker">
  self.onmessage = (e) => {
    self.postMessage('msg');
  };
</script>
<script>
  const blob = new Blob([
    document.querySelector('#worker').textContent
  ];

  const worker = new Worker(window.URL.createObjectURL(blob));
  worker.onmessage = (e) => {
    console.log(e.data);
  }
  worker.postMessage("hello");
</script>
```

通过获取 script 元素以及使用 `textContent` 属性，我们得到了作为 Blob 对象的 worker（blob 变量）。

然后，我们使用 `Worker` 构造函数和 blob 变量来创建一个 Worker 对象（worker 变量）。

现在，我们就可以在 worker 线程里以及调用 worker 的 script 元素内写我们通常的 worker 代码了。

## <a id="trimfname"></a>如何去除字符串内的文件扩展名

从字符串去除文件的扩展名，我们可以使用 `replace` 方法。

例如，我们可以这样写：

```javascript
fileName.replace(/\.[^/.]+$/, "");
```

我们可以使用正则表达式的模式得到字符串中在 `.` 之后的子串。

然后，我们可以用空字符串替换子串。

在 Node 应用中，我们可以使用 `path` 模块。

为了得到文件名，我们可以这样写：

```javascript
const path = require('path');
const filename = 'foo.txt';
path.parse(filename).name;
```

`path.parse` 获得文件的路径，然后我们访问 `name` 属性得到文件名。

## <a id="mapparse"></a>为什么在 Array.prototype.map 里使用 parseInt 会返回 NaN？

之所以 `parseInt` 在数组实例的 `map` 方法中会返回 `NaN` ，是因为 `parseInt` 接收的参数与 `map` 回调接收的 3 个参数不相对应。

`parseInt` 将（字符串类型的）数字（本例中）作为第一个参数，并且将 基数（radix）作为第二个参数。

`map` 回调将数组项作为第一个参数，索引作为第二个参数，数组本身作为第三个参数。

因此，如果我们直接使用 `parseInt` 作为回调，那么索引就会被传递作为基数 ，这显然不合适。

这就是为什么我们会得到 `NaN` 。

当使用的基数不合理时，我们就会得到 `NaN` 。

因此，不要这么写：

```javascript
['1','2','3'].map(parseInt)
```

我们可以这样写：

```javascript
['1','2','3'].map(Number)
```

或者

```javascript
['1','2','3'].map(num => parseInt(num, 10))
```

## <a id="omit"></a>使用三目运算符时省略第二个表达式

如果我们有如下的三目表达式：

```javascript
x === 1 ? doSomething() : doSomethingElse();
```

但是，当 `x===1` 的结果是 `false` ，我们又不想调用 `doSomethingElse` 时，我们可以使用 `&&` 操作符替代。

举个例子，我们可以写：

```javascript
x === 1 && dosomething();
```

当 `x===1` 结果为 `true` 时，`dosomething` 就会被调用。

## <a id="yarn"></a>清除 Yarn 中的缓存

使用 `yarn cache clean` ，我们可以清除 yarn 中的缓存。

## <a id="innerText"></a>innerText 在 IE 中有效，但在其他浏览器中无效

`innerText` 是仅用于 IE 的属性，用于填充节点的文本内容。

为了在其他浏览器中做到同样的事情，我们可以设置 `textContent` 属性。

例如，我们这么写：

```javascript
const el = document.getElementById('foo');
el.textContent = 'foo';
```

## <a id="conclusion"></a>总结

通过调用 `preventDefault` 来阻止默认操作，以此我们可以取消双击后的文本选中。

同样，我们不应该将 `parseInt`  用于 `map` 的回调。

如果我们创建一个子构造函数，我们必须将它的构造函数的值设置为当前的对象。

通过这种方式，我们可以正确的检查实例。