---
title: 如何用gitbook搭建自己的文档整合平台
date: 2019-01-18 23:30:41
tags: ["gitbook", "文档", "工具"]
---

最近要整合团队的文档，然后输出到其他团队。然后很多接口并不是由我提供的，我也只能参照大家在 gitlab 上写的 wiki 来整理起来，有些文档变动我可能也不能及时收集，所以想找这样的工具，大家都能方便地编辑文档，而且最后整合的文档能比较齐全，样式也好看点。gitlab 提供的 wiki 功能其实也是挺好用的，但是大多没有整理好，一个仓库一个对应的文档，而且翻页查找什么的也不是特别方便。最后找了好多文档框架，比较好用的几个多是专门用来提供 api 文档的，组里的文档多是 markdown 格式的，不想进行大改。最后选定了 gitbook 来做这样一个整合。

我的博客其实是用 hexo 来搭建的，我觉得很方便了，hexo 如果直接部署到 github 或者 gitlab page 都是很方便的，但是公司内部文档，最好还是内部使用，加上公司的 gitlab 没有支持 gitlab page，问了下同事意见，有想要用 gitbook 的，所以最后选用了 gitbook。

大致的思路是建一个专门用来写文档的 gitlab repo，组内成员都可以直接克隆到本地，然后编写文档，build 之后推送到仓库，再写个脚本将静态资源推送到服务器电脑上，基本就能大家写完就能更新上。

#### 安装

作为前端，nodejs、npm 这些环境是必备的，所以安装 gitbook-cli 工具就是分分钟的事了。
首先，全局安装 gitbook-cli

```
$ npm install gitbook-cli -g
```

#### 创建一本书

新建一个文件夹，执行初始化命令：

```
$ gitbook init
```

这个命令会生成两个模板文件，一个 README.md，这个里面会看到一行 Introduction 字样，其实就是可以理解为封皮的地方。还有一个是 SUMMARY.md,仔细看这个文件内容：

```
# Summary

* [Introduction](README.md)
```

其实就是目录了，如果要有二级目录可以像这样写：

```
# Summary

* [Introduction](README.md)
  * [二级目录](README.md)
```

如果想把文档都分类一下，用文件夹来分门别类，大致可以这样：

```
# Summary

* [介绍](README.md)
* [后端](./backend/README.md)
  * [小程序接口 api](./backend/smartprograme.md)
  * [小程序接口 api](./backend/smartprograme.md)
* [前端规范](./frontend/README.md)
  * [js规范](./frontend/jsStandard.md)
```

#### 预览

可以在本地起个服务看看效果：

```
$ gitbook serve
```

#### 生成静态页面

```
$ gitbook build
```

执行之后生成的 \_book 文件夹内的内容就是个静态网站内容了，直接放到服务器上就可以了，首页就是 index.html。非常之方便。

#### 配置

在根目录下创建一个 book.json 的配置文件，就可以为你的 gitbook 添加一下插件了，可以配置主题、样式和一些格式插件，等等。

```
{

    // 样式风格配置格式
    "styles": {
        "website": "styles/website.css",
        "ebook": "styles/ebook.css",
        "pdf": "styles/pdf.css",
        "mobi": "styles/mobi.css",
        "epub": "styles/epub.css"
     },

    // 插件安装配置格式
    "plugins": [
      "emphasize",     // 为文字加上底色
      "-highlight",   // 加了减号的表示禁用一些默认的配置，这里禁用了高亮
      "tbfed-pagefooter" // 为页面添加页脚
      ],
    // 插件对应的配置
    "pluginsConfig": {
        "tbfed-pagefooter": {
        "copyright":"Copyright",
        "modify_label": "该文件修订时间：",
        "modify_format": "YYYY-MM-DD HH:mm:ss"
       }
     }
}
```
添加了这些插件后要执行安装命令，这样才安装插件成功。
```
$ gitbook install
```
其他的插件可以直接上 [gitbook 插件列表页面]("https://plugins.gitbook.com/")上去找一找。

#### 总结
整个基本的流程大致就是这样了，很是容易，因为我也尝试使用 vuepress 来搭建一个文档整合的平台，感觉操作起来还是不太习惯，所以推荐大家也用 gitbook 来搭建一下。组里的 iOS 小伙伴看了我搭完之后的成品，立马说要用来整合自己的笔记，说是发现了新大陆一般，以后整理自己的笔记方便多了。

#### 相关链接
[gitbook 安装和起步文档](https://github.com/GitbookIO/gitbook/blob/master/docs/setup.md)
[gitbook 插件列表页面](https://plugins.gitbook.com/)