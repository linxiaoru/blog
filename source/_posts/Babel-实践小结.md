---
title: Babel 实践小结
date: 2017-10-16 21:37:32
tags: ['Babel', '前端']
---
### Babel 是什么？
> Babel 是一个 JavaScript 编译器。

这是 [Babel 官网(中文)](http://babeljs.cn/)上的一句介绍。实际上，Babel 可以将 ES6 代码转为 ES5 代码，让你不用去考虑环境是否支持，而直接使用 ES6 来编写代码。

<!--more-->

### 使用 Babel
#### 文件目录结构
```
  - babel-learning
    + .babelrc
    + package.json
    + src
```

#### 配置 .babelrc
在文件根目录下创建 .babelrc 文件，这个文件用来设置转码规则和插件，
```
  {
    "presets": [],    // 设定转码规则
    "plugins": []     // 设定插件
  }
```
参考网上的 .babelrc 文件经常看到这样的配置：
```
  {
    "presets": [
      "es2015",
      "react",
      "stage-0"
    ],
    "plugins": []
  }
```
这里解释一下，这个配置文件是针对 Babel 6 的。Babel 6 做了一系列模块化，不像 Babel 5 一样把所有的内容都加载。比如需要编译 ES6，我们需要设置 presets 为 "es2015"，也就是预先加载 ES6 编译的相关模块，如果需要编译 jsx 使用 react，则需要预先加载 "react" 这个模块。ES7 不同阶段语法提案的转码规则（共有4个阶段），分别是 "stage-0"、"stage-1"、"stage-2"、"stage-3"。

#### 配置 package.json
初始时的文件内容：
```
  {
  "name": "babel-learning",   // 项目名称
  "scripts": {      
    "build": "babel src -d lib"   // 配置输出路径
  },
}

```
关于输出路径的配置：
```
# 将整个 src 文件夹下内容进行编译，输出为 lib 文件夹
$ babel src -d lib
# 或者
$ babel src --out-dir lib

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ babel example.js --out-file compiled.js
# 或者
$ babel example.js -o compiled.js
```

#### 安装依赖

根据 .babelrc 文件的配置需要执行的命令行如下：
```
# ES2015 转码规则
$ npm install --save-dev babel-preset-es2015

# react 转码规则
$ npm install --save-dev babel-preset-react

# ES7 不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

**ES7 不同阶段语法提案的转码规则含义又都是什么呢？**
- stage-0：
  包含 stage-1，stage-2 以及 stage-3 的所有功能，同时还另外支持如下 2 个功能插件：[transform-do-expressions](https://babeljs.io/docs/plugins/transform-do-expressions)、[transform-function-bind](https://babeljs.io/docs/plugins/transform-function-bind)
- stage-1:
  包含 stage-2 和 stage-3，还包含了下面 2 个插件：[transform-class-constructor-call ](http://babeljs.io/docs/plugins/transform-class-constructor-call) 和 [transform-export-extensions](http://babeljs.io/docs/plugins/transform-export-extensions)
- stage-2:
  包含 stage-3，以及插件：[syntax-dynamic-import](https://babeljs.io/docs/plugins/syntax-dynamic-import/)、[transform-class-properties](https://babeljs.io/docs/plugins/transform-class-properties/)、[~~transform-decorators~~](https://babeljs.io/docs/plugins/transform-decorators/)(–disabled pending proposal update (can use the legacy transform in the meantime))
- stage-3:
  包含插件：[transform-object-rest-spread](https://babeljs.io/docs/plugins/transform-object-rest-spread/)、[transform-async-generator-functions](https://babeljs.io/docs/plugins/transform-async-generator-functions/)

安装完成后，package.json 文件内容为：
```
{
  "name": "babel-learning",
  "scripts": {
    "build": "babel src -d lib"
  },
  "devDependencies": {
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-0": "^6.24.1"
  }
}
```

#### 安装 Babel 的命令行工具依赖包 babel-cli 
方法一：全局安装
```
$ npm install --g babel-cli
```
方法二：在项目中安装
```
# 进入项目目录
$ npm install --save-dev babel-cli
```
如果希望不同的项目安装不同版本的 Babel，可以选择方法二，在项目中安装，这样项目也不会对全局环境产生依赖。

#### 运行
在 src 文件夹下新建 index.js 文件，写入测试代码：
```JavaScript
  [1, 2, 3].map(x => x * 2);
```
运行 `$ npm run build`
可以看到文件目录下多了 lib 文件夹，打开后里面有 index.js 文件，内容为：
```JavaScript
  "use strict";

  [1, 2, 3].map(function (x) {
    return x * 2;
  });
```
这就是转码后的 ES5 文件。

### 总结
以上只是最简单的实践小结，关于 Babel 还有很多依赖包可以研究，下次可以来总结一下这些包都有哪些用处。

#### 相关参考链接
[Babel 入门教程](http://www.ruanyifeng.com/blog/2016/01/babel.html)
[如何区分Babel中的stage-0,stage-1,stage-2以及stage-3（一）](http://www.cnblogs.com/flyingzl/p/5504203.html)