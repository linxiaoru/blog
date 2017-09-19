---
title: react-native 热更新之 CodePush 使用小结
date: 2017-08-23 22:58:23
tags: ['React Native', 'CodePush', '前端']
---
#### 先从一个需求说起
需求是这样的：为满足线上版本紧急更新重要内容（包括 JS/HTML/CSS/IMAGE 等），使用 CodePush 进行热更新， 当有新的更新内容需要更新时，客户端弹出自定义的更新提示弹窗，用户可以选择点击立即更新进行更新，或者点击关闭按钮忽略这次更新。

<!--more-->
#### 如何实现
**CodePush 实现热更新基本流程：**
  1.安装 CodePush CLI
  2.创建一个CodePush 账号
  3.在CodePush服务器注册app
  4.在app上添加CodePush SDK，配置升级相关代码
  5.更新代码后，发布一个应用更新到服务器
  6.app收到升级推送
针对 React Native 的 CodePush 具体可以查看 [react-native-code-push 文档](https://github.com/Microsoft/react-native-code-push) 

**客户端检测更新并在用户操作之后进行相应的更新升级**
根据 CodePush 的 [Api 文档](https://github.com/Microsoft/react-native-code-push/blob/master/docs/api-js.md), 
> sync: Allows checking for an update, downloading it and installing it, all with a single call. Unless you need custom UI and/or behavior, we recommend most developers to use this method when integrating CodePush into their apps

sync 方法能够检测更新，下载并安装，而且这个方法也是比较推荐的方法，但是需求中说了，需要自定义的弹窗提示，所以这一个方法并不是能够直接可行的。这个时候，就需要再看下文档中提供的另一个方法了 checkForUpdate 方法，这个方法用来检测是否有可用更新，具体实现过程：
- 在 App 首页（方便显示自定义弹窗） ComponentDidMount 中使用 checkForUpdate 检测是否有更新内容即可，示例：

```javascript
codePush.checkForUpdate()
.then((update) => {
    if (!update) {
        console.log("The app is up to date!");  // 没有可用的更新
    } else {
        console.log("An update is available! Should we download it?"); // 有可用的更新
        this.setState({   
          modelVisiable: true   // 设置 Modal 弹窗可见
        })
    }
});
```
- 更新弹窗可见后，点击立即更新按钮
```javascript
  <TouchableOpacity onPress={this.updateNow.bind(this)}>
    <Text style={styles.updateButton}>立即更新</Text>
  </TouchableOpacity>
```
- 用户选择立即更新之后，开始进行更新下载
这个时候，就能使用 sync 方法了，下载并安装，sync 方法提供一个 Dialog 选项，这是用来自带的类似 confirm 弹窗，我们已经有弹窗了，就不需要了, installMode 参数就是安装模式，我选择 CodePush.InstallMode.IMMEDIATE，为了展示更新进度，就需要两个方法
```javascript
  syncImmediate() {
    CodePush.sync(
      { installMode: CodePush.InstallMode.IMMEDIATE },
      this.codePushStatusDidChange.bind(this),
      this.codePushDownloadDidProgress.bind(this)
    );
  }
```

codePushStatusDidChange 用来跟踪更新状态，codePushDownloadDidProgress 则是更新的进度，实例

```javascript
 codePushStatusDidChange(syncStatus) {
    switch(syncStatus) {
      case CodePush.SyncStatus.CHECKING_FOR_UPDATE:
        this.setState({ syncMessage: "检测更新" });
        break;
      case CodePush.SyncStatus.DOWNLOADING_PACKAGE:
        this.setState({ syncMessage: "下载啦" });
        break;
      case CodePush.SyncStatus.AWAITING_USER_ACTION:
        this.setState({ syncMessage: "等用户操作" });
        break;
      case CodePush.SyncStatus.INSTALLING_UPDATE:
        this.setState({ syncMessage: "开始安装" });
        break;
      case CodePush.SyncStatus.UP_TO_DATE:
        this.setState({ syncMessage: "更新好了啦", progress: false });
        break;
      case CodePush.SyncStatus.UPDATE_IGNORED:
        this.setState({ syncMessage: "用户取消了更新", progress: false });
        break;
      case CodePush.SyncStatus.UPDATE_INSTALLED:
        this.setState({ syncMessage: "安装好了都，重新启动 App 就生效", progress: false });
        break;
      case CodePush.SyncStatus.UNKNOWN_ERROR:
        this.setState({ syncMessage: "出错了", progress: false });
        break;
    }
  }

  codePushDownloadDidProgress(progress) {
    this.setState({ progress });  // progress 有两个属性，当前接收到的数据 receivedBytes 和总的数据量 totalBytes,
    
    let percent = parseInt(progress.receivedBytes / progress.totalBytes * 100);
    this.setState({
      codePushProgress: '更新进度:' + percent + '%',    // 为了显示进度百分比
    });
  }
```
- 下载完成
下载完成之后，必须要重新启动 App ,当前的更新才能生效，这时我们已经可以在 codePushStatusDidChange 中得到是否安装好了的状态，当状态为 CodePush.SyncStatus.UPDATE_INSTALLED, 只要适时地在当前的弹窗中显示一个按钮，让用户点击之后重启 App 就好，CodePush 也提供了重启 App 的方法
```javascript
 CodePush.restartApp();
```

#### 完成
以上就是我完成这个需求的思路及方法，仔细阅读文档，查看文档实例，总会有很多的实现的方法，随着之后的项目代码优化，可能有更好的实现方式。

#### 方案优化
2017.09.18 针对需求不变的情况下，在使用了 `checkForUpdate（）` 方法后，获取是否有新的内容需要更新，当有需要更新时，弹出更新提示，然后等待用户点击立即更新按钮，这个时候可以直接使用 `download()` 方法，下载更新包，更新完毕后，用户点击立即重启完成更新按钮后，`install` 安装包。

这个方案主要是取消了使用 `sync()` 方法，`download（）` 方法中同样也可以获取下载进度，也不再去根据状态来判断是否更新完成，实例修改后如下
```javascript
componentWillMount（）{
 CodePush.checkForUpdate(Config.codePushKey)
          .then((update) => {
            this.apk_package = update;  // 更新状态等信息
            if (update) {
              // 有可用的更新，这时进行一些控制更新提示弹出层的的操作
            }else{
              // 没有可用的更新
            }
          });
}

  _updateCodePush = () => {
    this.apk_package.download((progress) => {
      // 更新进度换算
      let percent = parseInt(progress.receivedBytes / progress.totalBytes * 100);
      this.setState({
        codePushProgress: '更新进度:' + percent + '%',
      });     
    }).then((localPackage)=>{
      this.apk_localPackage = localPackage;   // 下载完毕了，这时候已经获取了更新包
      // 如若不需要用户进行是否立即重启生效更新的操作，可以直接进行 install
    });
  }

  _installPagage = () => {
    // 待用户同意立即重启以生效更新后，进行 install 操作，install 方法内置了安装后立即重启的方法
     this.apk_localPackage.install(CodePush.InstallMode.IMMEDIATE);
  }
```
### CodePush 常用命令
1. 查看更新历史

    > code-push deployment h myApp Staging
    或者    
    > code-push deployment history myApp Production

2. 查看应用key

    > code-push deployment ls myApp -k
    或者
    > code-push deployment list myApp -k

3. 与账号相关的命令
    - 在账号里面添加一个新的app
        > code-push app add myNewApp

    - 在账号里移除一个app
        > code-push app remove myApp        
        或者
        > code-push app rm myApp

    - 重命名一个存在app
        > code-push app rename myApp myAppNewName

    - 列出账号下面的所有app
        > code-push app list
        或者
        > code-push app ls

    - 把app的所有权转移到另外一个账号
        > code-push app transfer
4. 推送更新内容

    code-push release-react <appName> <platform>

示例(默认走```Staging```分支):
> code-push release-react MyApp ios    
> code-push release-react MyApp-Android android
 
指定部署分支名称 
> code-push release-react MyApp android -d Production

指定开发版本或发布版本(默认false)
> code-push release-react MyApp windows --dev

强制更新描述内容 
> code-push release-react MyApp ios -m --description "Modified the header color"

更新一个1/4用户的开发版本
> code-push release-react MyApp-Android android --rollout 25% --dev true

更新1.1.0版本的所有用户
> code-push release-react MyApp-Android android --targetBinaryVersion "~1.1.0" 
或者  
> code-push release-react MyApp-Android android --t "~1.1.0"


#### 参考链接
- [Code Push 热更新使用详细说明和教程](http://bbs.reactnative.cn/topic/725/code-push-%E7%83%AD%E6%9B%B4%E6%96%B0%E4%BD%BF%E7%94%A8%E8%AF%A6%E7%BB%86%E8%AF%B4%E6%98%8E%E5%92%8C%E6%95%99%E7%A8%8B)

- [react-native-code-push 文档](https://github.com/Microsoft/react-native-code-push)