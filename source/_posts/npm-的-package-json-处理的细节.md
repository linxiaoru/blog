---
title: '[翻译]npm 的 package.json 处理的细节'
date: 2018-12-24 22:44:51
tags: ['翻译', '前端', 'npm']
---

为了更细致地了解 package.json 中每个字段，特意将官方文档翻译了一下，加入了一点点自己经验和理解。
文档地址：[npm-package.json](https://docs.npmjs.com/files/package.json)

## 说明

本文档是你所需要了解的关于你的 paclage.json 文件中你需要什么的全部信息。package.json 必须是真正的 JSON，而不仅仅是 JavaScript 对象字面量。
本文中描述的许多行为都受 npm-config 中描述的配置设置的影响。
<!--more-->

### name

如果你计划发布你的 `package`，你的 package.json 文件中最重要的内容就是 `name` 和 `version` 字段，因为他们是必需的字段。`name`（包名） 和 `version`（版本号） 共同组成了一个被认为是完全唯一的标识符。package 的更改应该伴随着版本号的更改。如果你不计划发布你的 package，`name` 和 `version` 字段是可选的。
`name` 就是你的 package 包名。
一些规则：

- name 必须小于等于 214 个字节，包括 scoped package 的 socpe 名在内。（例如：@xxx/xxx-package)。
- name 不能以 "." 或 "\_" 开头。
- name 中不能含有大写字母。
- 该名称最终成为 URL 的一部分，命令行上的参数和文件夹名称。因此，名称不能包含任何非 URL 安全字符。
  一些提示：
- 不要使用和 Node 核心模块同名的名字。
- 不要将 "js" 或 "node" 放入名称中。它在你编写 package.json 文件时就已经被假设为一个 js 文件了，同时，你可以指定你使用的引擎通过 "engine" 字段（查看下文）。
- 这个名字可能会被当作一个参数传递给 require()，因此它应该是简短的，但是也是合理的描述性，语义化的。
- 在取名字之前，你可以查看 npm 网站看看名字是否被占用了。[查看网址](https://www.npmjs.com/)
  名字可以选择以 socpe 作为前缀，例如：@myorg/mypackage。详情请查 [npm-scope](https://docs.npmjs.com/misc/scope)

### version

如果你计划发布你的 `package`，你的 package.json 文件中最重要的内容就是 `name` 和 `version` 字段，因为他们是必需的字段。`name`（包名） 和 `version`（版本号） 共同组成了一个被认为是完全唯一的标识符。package 的更改应该伴随着版本号的更改。如果你不计划发布你的 package，`name` 和 `version` 字段是可选的。
版本号必须可以被 [node-semver](https://github.com/npm/node-semver) 解析，node-semer 是被作为依赖项与 npm 捆绑在一起的（你可以使用 `npm install semver` 来使用它）
关于 semver 版本号和范围的信息可以查看 [npm-semver](https://docs.npmjs.com/misc/semver)。

### description

`description` 字段用于添加描述。是一个字符串。这有助于人们发现你的 package，当使用 `npm search` 时，它会在其中列出这个信息。
使用 `npm search create-react-cli` 所得到的信息，列出了所有相关的包，其中主要显示了 `name`、`description` 、`author`、`date`、`version` 和 `keywords` 信息。
[img](./npmsearch.png)

### keywords

`keywords` 用于放置关键字。它可以是一个字符串数组。这有助于人们发现你的 package，当使用 `npm search` 时，它会在其中列出这个信息。

### homepage

项目主页的 url。
示例：

```json
"homepage": "https://github.com/owner/project#readme"
```

### bugs

项目 issue 提交的地址 url 和/或报告问题的 email 地址。这些对那些使用了你的包但是遇到问题的人很有帮助。
它看起来像这样：

```json
{
  "url": "https://github.com/owner/project/issues",
  "email": "project@hostname.com"
}
```

你可以指定一个或两个值。 如果只想提供 url，可以将 "bugs" 的值指定为简单字符串而不是对象。
如果提供了 url，`npm bugs` 命令将会使用这个 url。

### license

你应该为你的包指定一个许可证，这样人们就知道他们是否允许使用这个 package，以及你对你的 package 的任何限制。
如果你使用的是 BSD-2-Clause 或 MIT 等常用许可证，请为你正在使用的许可证添加当前的 SPDX 许可证标识符，如下所示：

```json
{ "license": "BSD-3-Clause" }
```

你可以查看 [SPDX 许可证标识的完整列表](https://spdx.org/licenses/)。理想情况下，你应该挑选一个 [OSI](https://opensource.org/licenses/alphabetical) 认可的。
如果你的 package 在多个常见许可下获得许可，请使用 SPDX 许可表达式语法 2.0 版字符串，如下所示：

```json
{ "license": "(ISC OR GPL-3.0)" }
```

如果你使用的许可证尚未分配 SPDX 标识符，或者你使用的是自定义许可证，请使用如下字符串值:

```json
{ "license": "SEE LICENSE IN <filename>" }
```

然后在包的顶层包含一个名为 <filename> 的文件。
一些老的包使用许可证对象或包含许可证对象数组的 "licenses" 属性：

```json
// Not valid metadata
{
  "license" :
    {
      "type" : "ISC", 
      "url" : "https://opensource.org/licenses/ISC"
    }
}
// Not valid metadata
{ 
  "licenses" :
  [
    { 
      "type": "MIT",
      "url": "https://www.opensource.org/licenses/mit-license.php"
    }, 
    { 
      "type": "Apache-2.0", 
      "url": "https://opensource.org/licenses/apache2.0.php"
    }
  ]
}
```

这些风格的定义方式已经被弃用了。取而代之的是 SPDX 表达式，像这样：

```json
{ "license": "ISC" }
{ "license": "(MIT OR Apache-2.0)" }
```

最后，如果你不希望授予他人任何条款下使用私有的或未发布包的权利：

```json
{ "license": "UNLICENSED" }
```

还可以考虑设置 "private": true 以防止意外发布

### 和人员有关的字段：author，contributors

"author" 是一个开发人员，"contributors" 是一个开发人员数组。"author"是一个拥有 "name" 字段和可选的 "url" 和 "email" 字段的对象，像这样：

```json
{
  "name": "Barney Rubble",
  "email": "b@rubble.com",
  "url": "http://barnyrubble.tumblr.com/"
}
```

或者，你可以将这些缩短为一个字符串，npm 会为你解析它:

```json
"Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"
```

email 和 url 都是可选的。
npm 还用你的 npm 用户信息设置顶级“维护者”字段。

### files

可选的 files 字段是一个文件模式数组，描述了将 package 作为依赖项安装时要包含的条目。文件模式遵循与 .gitignore 类似的语法。但是相反的是：包含一个文件，目录，或者 glob 模式(_，\*\*/_，和等等)将会设置为使文件在打包时包含在 tarball 中。省略该字段，将会默认设置为 `[“*”]`，这意味着它将会包含所有文件。
一些特殊的文件或者目录会被包括或者排除不管他们是否存在在 files 数组中（详见下文）
你可以在 package 根目录或者子目录中提供一个 .npmignore 文件，这样可以防止文件被包含在内。在 package 根目录下它不会覆盖 files 字段，但是在子目录中他将会覆盖 files 字段。.npmignore 文件像 .gitignore 文件一样工作。但是如果缺省 .npmignore 文件，则将使用 .gitignore 的内容。
"package.json＃files" 字段中包含的文件不能通过 .npmignore 或 .gitignore 排除。
某些文件始终包括在内，无论设置如何：

- package.json
- README
- CHANGES / CHANGELOG / HISTORY
- LICENSE / LICENCE
- NOTICE
- main 字段中包含的文件
  README, CHANGES, LICENSE & NOTICE 可以包含任何文件扩展名。
  相反地，某些文件总是被忽略：
- .git
- CVS
- .svn
- .hg
- .lock-wscript
- .wafpickle-N
- .\*.swp
- .DS_Store
- .\_\*
- npm-debug.log
- .npmrc
- node_modules
- config.gypi
- \*.orig
- package-lock.json (使用 npm shrinkwrap 来替代管理依赖)

### main

main 字段是模块的 ID，是程序的主要入口。也就是说，如果你的包名为 **foo**，然后用户安装了它，并通过 `require("foo")` 使用了这个模块，那么 require 返回的内容就是 main 字段指定的文件中的模块返回的对象。
这应该是一个相对于包文件夹根目录的模块 ID。
对于大多数模块来说，拥有一个 main 字段指定的主要入口是最有意义的，通常不会有太多其他内容。

### browser

如果您的模块要在客户端使用，则应使用浏览器字段而不是 `main`字段。 这有助于提示用户它可能依赖于 Node.js 模块中不可用的基元。 （例如窗口）

### bin

许多包都有一个或多个可以安装到 PATH 中的可执行文件。npm 让这个工作变得很简单（事实上，它自己也是使用这个功能特性来安装 "npm" 可执行文件）
要使用它，请在 package.json 中提供 bin 字段，该字段是命令名到本地文件名的映射。 在安装时，npm 会将该文件符号链接到用于全局安装的 prefix(前缀)/bin，或者用于本地安装的 ./node_modules/.bin/。
举个例子：

```json
{ "bin": { "myapp": "./cli.js" } }
```

因此，当你安装 myapp 时，它会创建一个从 cli.js 脚本到 /usr/local/bin/myapp 的符号链接。
如果您有一个可执行文件，并且其名称应该是包的名称，那么您可以将其作为字符串提供。 例如：

```json
{ "name": "my-program", "version": "1.2.5", "bin": "./path/to/program" }
```

等价于：

```json
{
  "name": "my-program",
  "version": "1.2.5",
  "bin": { "my-program": "./path/to/program" }
}
```

请确保你在 bin 字段引用的文件内部是以 `#!/usr/bin/env node` 为开头，否则的话脚本将在没有 node 的情况下被执行。（也就是说不会自动地用 node 执行该程序，这个命令是指定脚本的解释程序语句。）

### man

指定单个文件或一个文件名数组以供 linux 下的 `man` 程序查找文档地址。
如果只提供了一个文件，那么它安装后就是直接使用 `man <pkgname>`，而不管它的实际文件名是什么。 例如：

```json
"name" : "foo",
"version" : "1.2.3",
"description" : "A packaged foo fooer for fooing foos",
"main" : "foo.js",
"man" : "./man/doc.1"
```

执行 `man foo` 会链接到 `./man/doc.1` 文件。
如果文件名没有以包名开头，它会自动添加前缀。因此：

```json
"name" : "foo",
"version" : "1.2.3",
"description" : "A packaged foo fooer for fooing foos",
"main" : "foo.js",
"man" : [ "./man/foo.1", "./man/bar.1" ]
```

这将会为 `man foo` 和 `man foo-bar` 创建文件。
man 文件必须以数字结尾，如果压缩，则必须以 `.gz` 后缀结尾。 该数字指示文件安装到哪个 man 部分。

```json
"name" : "foo",
"version" : "1.2.3",
"description" : "A packaged foo fooer for fooing foos",
"main" : "foo.js",
"man" : [ "./man/foo.1", "./man/foo.2" ]
```

将为 `man foo` 和 `man 2 foo` 创建两条命令。

### directories

CommonJS Packages 规范详细介绍了使用目录对象指示包结构的几种方法。 如果你看一下 npm 的 package.json，你会看到它有 doc，lib 和 man 的目录。
将来，此信息可能会以其他创造性方式使用。

#### directories.lib

告诉人们你的 lib 文件夹在哪。 lib 文件夹没有任何特殊功能，但它是有用的元信息。

#### directories.bin

如果你在这里指定了 bin 目录，这个配置下面的文件会被加入到 bin 路径下。
由于 bin 指令的工作方式，指定 bin 路径和设置 directories.bin 是错误的。如果要指定单个文件，请使用 bin，对于现有 bin 目录中的所有文件，请使用 directories.bin。

#### directories.man

directories.man 指定的文件夹里都是 man 文件，通过遍历这个文件夹来生成一个 man 的数组，是一个语法糖。

#### directories.doc

将 markdown 文件放在这里。 最终，这些将会很好地显示，也许有一天会显示出来。

#### directories.example

把示例脚本放在这。总有一天，它可能会以某种巧妙的方式展示出来。

#### directories.test

把你的测试用例放在这里。目前还没有公开，但将来可能会公开。

### repository

指明你的代码被托管在何处，这对那些想要参与到这个项目中的人来说很有帮助。如果 git 仓库在 github 上，用 `npm docs` 命令将会找到你。
像这样：

```json
"repository": {
  "type" : "git",
  "url" : "https://github.com/npm/cli.git"
}
"repository": {
  "type" : "svn",
  "url" : "https://v8.googlecode.com/svn/trunk/"
}
```

url 应该是公开且可用的(可能是只读的)，这个 url 应该可以被版本控制系统不经修改地处理。不应该是一个在浏览器中打开的 html 项目页面，这个 url 是给计算机使用的。
对于 GitHub，GitHub gist，Bitbucket 或 GitLab 存储库，您可以使用与 npm install 相同的快捷语法：

```json
"repository": "npm/npm"
"repository": "github:user/repo"
"repository": "gist:11081aaa281"
"repository": "bitbucket:user/repo"
"repository": "gitlab:user/repo"
```

### scripts

“scripts” 属性是一个包含脚本命令的字典，这些命令在包的生命周期中的不同时间运行。 key 是生命周期事件，value 是运行的命令。
请参阅[npm-scripts](https://docs.npmjs.com/misc/scripts)以了解有关编写 scripts 的更多信息

### config

“config” 对象可用于设置 scripts 中使用的配置参数，这些参数在升级过程中保持不变。例如，如果包具有以下内容:

```jaon
{
  "name" : "foo",
  "config" : { "port" : "8080" }
}
```

然后有一个 “start” 命令，然后引用 **npm_package_config_port** 环境变量，然后用户可以通过执行 `npm config set foo：port 8001` 来覆盖它。
有关包配置的更多信息，请参阅 [npm-config](https://docs.npmjs.com/misc/config) 和 [npm-scripts](https://docs.npmjs.com/misc/scripts)。

### dependencies

`dependencies` 是一个对象，指定了依赖项包名和其版本范围的映射。版本范围是一个字符串，其中包含一个或多个空格分隔的描述符。`dependencies` 也可以使用压缩包或 git URL 来识别依赖关系。
不要把测试工具或 transpilers 写到 dependencies 中。请参阅下面的 devDependencies。
有关指定版本范围的更多详细信息，请参阅 [semver](https://docs.npmjs.com/misc/semver)。

- **version** 必须与 version 完全匹配
- **\>version** 必须高于 version
- **\>=version** 必须大于等于 version
- **\<version** 必须小于 version
- **<=version** 必须小于等于 version
- **～ version** 约等于 version，具体参阅 [semver](https://docs.npmjs.com/misc/semver)
- **^version** 兼容 version，具体参阅 [semver](https://docs.npmjs.com/misc/semver)
- **1.2.x** 1.2.0，1.2.1，等等，但不能是 1.3.0，就是 1.2.几都行
- **http:// ...** 具体查看下面的“URLs as Dependencies”
- **\*** 匹配任何版本
- **""** (空字符串)和 \* 一样匹配任何版本
- **version1 - version2** 和 **>=version1 <=version2**一样
- **range1 || range2** 如果满足 range1 或 range2，就通过
- **git...** 具体查看下面的“Git URLs as Dependencies”
- **user/repo** 具体查看下面的“GitHub URLs”
- **tag** 一个以 tag 发布的指定版本，具体查看[npm-dist-tag](https://docs.npmjs.com/cli/dist-tag)
- **path/path/path** 查看下面的”Local Paths“
  举个例子，下面的这些写法都是合法的：

```json
{
  "dependencies": {
    "foo": "1.0.0 - 2.9999.9999",
    "bar": ">=1.0.2 <2.1.2",
    "baz": ">1.0.2 <=2.3.4",
    "boo": "2.0.1",
    "qux": "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0",
    "asd": "http://asdf.com/asdf.tar.gz",
    "til": "~1.2",
    "elf": "~1.2.3",
    "two": "2.x",
    "thr": "3.3.x",
    "lat": "latest",
    "dyl": "file:../dyl"
  }
}
```

#### URLs as Dependencies

可以在 version 上指定一个压缩包的 url。
当执行 npm install 时这个压缩包会被下载并且安装到本地。

#### Git URLs as Dependencies

git url 格式如下：

```json
<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish> | #semver:<semver>]
```

**\<protocol\>** 可以是 **git**，**git+ssh**，**git+http**，**git+https**，或 **git+file**其中之一。
如果提供了 **＃<commit-ish\>**，它将用于完全克隆该提交。那么它将会用来精确地克隆该 commit。如果 commit-ish 的格式为 **#semver:\<semver\>**，**\<semver\>** 可以是任何有效的 semver 范围或精确版本，npm 将在远程存储库中查找与该范围匹配的任何标签或引用，就像查找注册表相关性一样。 如果既未指定 **＃<commit-ish\>** 或 **\#semver：\<semver\>**，则使用 **master**。
示例：

```json
git+ssh://git@github.com:npm/cli.git#v1.0.27
git+ssh://git@github.com:npm/cli#semver:^5.0
git+https://isaacs@github.com/npm/cli.git
git://github.com/npm/cli.git#v1.0.27
```

#### GitHub URLs

从 1.1.65 版本开始，你可以引用 Github urls 作为版本号，比如”foo”: “user/foo-project”。也可以包含一个 commit-ish 后缀，举个例子：

```json
{
  "name": "foo",
  "version": "0.0.0",
  "dependencies": {
    "express": "expressjs/express",
    "mocha": "mochajs/mocha#4727d357ea",
    "module": "user/repo#feature/branch"
  }
}
```

#### Local Paths

从版本 2.0.0 开始你可以提供包含包的本地目录的路径。本地路径可以在你使用 **npm install -S** 或 **npm install –save** 时被保存，具体形式如下：

```json
../foo/bar
~/foo/bar
./foo/bar
/foo/bar
```

在这种情况下它会被标准化为一个相对路径并添加到你的 package.json 文件中，例如：

```json
{
  "name": "baz",
  "dependencies": {
    "bar": "file:../foo/bar"
  }
}
```

此功能有助于本地离线开发和创建需要在不想访问外部服务器的情况下进行 npm 安装的测试，但在将包发布到公共注册表时不应使用此功能。

### devDependencies

如果有人计划在他们的程序中下载和使用你的模块，那么他们可能不希望或不需要下载和构建你使用的外部测试或文档框架。
在这种情况下，最好将这些附加项放到 **devDependencies** 对象中。
这些东西将在从包的根目录执行 **npm link** 或 **npm install** 时安装，并且可以像任何其他 npm 配置参数一样进行管理。 有关该主题的更多信息，请参阅 [npm-config](https://docs.npmjs.com/misc/config)。
对于非特定于平台的构建步骤，例如将 CoffeeScript 或其他语言编译为 JavaScript，请使用 **prepare** 脚本执行此操作，并使所需的包成为 devDependency。
举个例子：

```json
{
  "name": "ethopia-waza",
  "description": "a delightfully fruity coffee varietal",
  "version": "1.2.3",
  "devDependencies": {
    "coffee-script": "~1.6.3"
  },
  "scripts": {
    "prepare": "coffee -o lib/ -c src/waza.coffee"
  },
  "main": "lib/waza.js"
}
```

**prepare** 脚本将在发布之前运行，以便用户可以使用该功能而无需他们自己编译。 在开发模式下（即本地运行 **npm install**），它也将运行此脚本，以便您可以轻松地进行测试。

### peerDependencies

有时候你希望表示程序包与主机工具或库的兼容性，而不一定要求此主机。就是做一些插件开发，它们往往是在核心依赖库的某个版本的基础上开发的，比如开发 react-native 插件，而在他们的代码中并不会出现 require("react-native") 这样的依赖，dependencies 配置里边也不会写上 react-native 的依赖，为了说明此模块只能作为插件跑在宿主的某个版本范围下，可以配置 peerDependencies：
举个例子：

```json
{
  "name": "tea-latte",
  "version": "1.3.5",
  "peerDependencies": {
    "tea": "2.x"
  }
}
```

这样可以确保你的包 **tea-latte** 可以与 **tea** 的第二个主要版本一起安装。 **npm install tea-latte** 可能会产生以下依赖图：

```json
├── tea-latte@1.3.5
└── tea@2.2.0
```

注: 如果它们在依赖关系树中没有明确依赖于更高版本，npm 版本 1 和版本 2 将自动安装 peerDependencies。在下一个主要版本的 npm ( npm@3 )中，情况将不再如此。你将收到警告，提示你没有安装 peerDependency。npms 1 和 2 中的行为经常令人困惑，很容易让你陷入依赖地狱，npm 是为了尽可能避免这种情况。
尝试安装具有冲突要求的另一个插件将导致错误。 因此，请确保您的插件要求尽可能广泛，而不是将其锁定到特定的修补程序版本。
假设 host 符合 semver，只有 host 包主版本的更改才会破坏您的插件。 因此，如果您使用过 1.x 版本的每个包，请使用“**^ 1.0**”或“**1.x**”来表达此信息。 如果您依赖于 1.5.2 中引入的功能，请使用“**\> =1.5.2 <2**”。

### bundledDependencies

这定义了发布包时要捆绑的包名数组。
如果您需要在本地保存 npm 包或通过单个文件下载使它们可用，您可以通过在 **bundledDependencies** 数组中指定包名并执行 **npm pack**，将包打包到压缩文件中。
举个例子：
如果我们像这样定义 package.json：

```json
{
  "name": "awesome-web-framework",
  "version": "1.0.0",
  "bundledDependencies": ["renderized", "super-streams"]
}
```

我们可以在执行 **npm pack** 后得到一个 **awesome-web-framework-1.0.0.tgz** 文件。此文件包含依赖项 **renderized** 和 **super-streams**，可以通过执行 **npm install awesome-web-framework-1.0.0.tgz** 将其安装在新项目中。
如果这里拼写作 “**bundleDependencies**”，也是可以的。

### optionalDependencies

如果可以使用依赖项，但是如果无法找到或无法安装，您希望 npm 继续，那么您可以将它放在 **optionalDependencies** 对象中。 这是包名称到版本或 URL 的映射，就像 **dependencies** 对象一样。 不同之处在于构建失败不会导致安装失败。
处理依赖性的缺失仍然是你的程序的责任。例如，像这样：

```js
try {
  var foo = require("foo");
  var fooVersion = require("foo/package.json").version;
} catch (er) {
  foo = null;
}
if (notGoodFooVersion(fooVersion)) {
  foo = null;
}
// .. then later in your program ..
if (foo) {
  foo.doFooThings();
}
```

**optionalDependencies** 中的条目将覆盖 **dependencies** 中相同名称的条目，因此通常最好只放在一个位置。

### engines

你可以指定你的项目运行环境所使用的 node 的版本：

```json
{ "engines": { "node": ">=0.10.3 <0.12" } }
```

并且，与 dependencies 一样，如果您没有指定版本（或者如果您指定“\*”作为版本），那么任何版本的节点都可以。
如果你指定了一个 “engines” 字段，则 npm 将会在某处包含这个 node 版本。如果忽略 “engines” 字段，则 npm 只会仅仅假设这个包工作在 node 下。
您还可以使用 “engines” 字段指定哪些版本的 npm 能够正确安装您的程序。 例如:

```json
{ "engines": { "npm": "~1.0.20" } }
```

除非用户设置了 **engine-strict** 配置标志，否则此字段仅供参考，并且仅在将在你的包安装为依赖项时才会产生警告。

### engineStrict

此功能已在 npm 3.0.0 中删除
npm 3.0.0 之前的版本，这个特性用来处理那些设置了 engine-strict 标记的包。

### OS

你可以指定模块将在哪些操作系统上运行:

```json
"os" : [ "darwin", "linux" ]
```

你也可以使用操作系统黑名单来替代白名单，只要在前面加个’!’：

```json
"os" : [ "!win32" ]
```

主机操作系统由 **process.platform** 确定
虽然没有任何理由这样做，但它允许黑名单和白名单。

### cpu

如果你的代码只在某些 cpu 架构上运行，你可以指明是哪些个。

```json
"cpu" : [ "x64", "ia32" ]
```

和 **os** 一样，你可以使用黑名单：

```json
"cpu" : [ "!arm", "!mips" ]
```

cpu 架构可以由 **process.arch** 确定

### preferGlobal

**已废弃**
此选项用于触发 npm 警告，但它不会再发出警告了。 它纯粹是为了提供信息。 现在建议您尽可能将二进制文件安装为本地 devDependencies。

### private

如果你在 package.json 中设置 **"private": true**，那么 npm 就会拒绝发布它。
这个一个防止意外发布私有仓库的方法。如果你希望确保给定的包只发布到特定的注册表（例如，内部注册表），那么使用下面描述的 **publishConfig** 字段在发布时覆盖 **registry** 配置参数。

### publishConfig

这是一组在发布时将会用的配置集合。如果你想要设置 tag，registry 或 access，它会非常的方便，以便你可以确保给定的包没有被打上”latest“的 tag 时，不会发布到全局公共 registry，或默认情况下， scoped 模块是私有的。
可以覆盖任何配置值，但只有 “tag”，“registry” 和 “access” 可能对于发布而言很重要。
请参阅 [npm-config](https://docs.npmjs.com/misc/config) 以查看可以覆盖的配置选项列表。

## 默认值

npm 将根据包内容设置一些默认值。

- **"scripts": {"start": "node server.js"}**
  如果在你的包的根目录下有一个 **server.js** 文件，那么 npm 将会设置默认的 **start** 命令为 **node server.js**
- **"scripts":{"install": "node-gyp rebuild"}**
  如果你的包的根目录下有一个 **binding.gyp** 文件并且你没有定义一个 **install** 或 **preinstall** 脚本，npm 将默认使用 node-gyp 进行编译的 **install** 命令。
- **"contributors": [...]**
  如果在你的包的根目录下有一个 **AUTHORS** 文件，npm 会将每一行视为 **Name <email>（url）** 格式，其中 email 和 url 是可选的。 以＃开头或为空的行将被忽略。

## 参见

- [semver](https://docs.npmjs.com/misc/semver)
- [npm-init](https://docs.npmjs.com/cli/init)
- [npm-version](https://docs.npmjs.com/cli/version)
- [npm-config](https://docs.npmjs.com/cli/config)
- [npm-config](https://docs.npmjs.com/misc/config)
- [npm-help](https://docs.npmjs.com/cli/help)
- [npm-install](https://docs.npmjs.com/cli/install)
- [npm-publish](https://docs.npmjs.com/cli/publish)
- [npm-uninstall](https://docs.npmjs.com/cli/uninstall)
