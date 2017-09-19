---
title: react-native 不定宽高网络图片解决方案
date: 2017-08-28 22:39:12
tags: ['React Native', '前端']
---
#### 先从一个需求说起
这次的需求是这样的：有一个展示页面，需要加载一些网络图片，而且是要让图片按原来的图片大小等比例展示的，，但是这些网络图片是不预先知道其长宽的。

<!--more-->
#### 如何实现
React Native 中 Image 组件，可以展示静态图片资源，也可以展示网络图片资源，从0.14版本开始，React Native提供了一个统一的方式来管理iOS和Android应用中的图片。要往App中添加一个静态图片，只需把图片文件放在代码文件夹中某处，然后像下面这样去引用它：
```javascript
<Image source={require('./my-icon.png')} />
```
它还可以使用@2x，@3x这样的文件名后缀，来为不同的屏幕精度提供图片，具体使用方法可以查看文档。

而对于网络加载的图片呢，还需要手动指定图片的尺寸，要像下面这样使用：
```javascript
// 正确
<Image source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}}
       style={{width: 400, height: 400}} />

// 错误
<Image source={{uri: 'https://facebook.github.io/react/img/logo_og.png'}} />
```
以上这样不去指定图片的尺寸，会在设备上无法显示图片。

那么问题就来了，网络图片，如果我需要按原图比例展示图片要如何实现呢？

#### 具体实现方法
- 方法一：Image 组件提供了 getSize 方法，可以在显示图片前获取图片的宽高(以像素为单位)
```javascript
static getSize(uri: string, success: (width: number, height: number) => void, failure: (error: any) => void) 
```
根据这个方法，写个组件来获取图片宽高并按比例展示：
```javascript
import React, { Component, PropTypes } from 'react';
import { Image } from 'react-native';

const propTypes = {
  uri: PropTypes.string.isRequired,
  width: PropTypes.number,
  height: PropTypes.number
};

export default class ScaledImage extends Component {
  constructor(props) {
    super(props);
    this.state = { source: { uri: this.props.uri } };
  }

  componentWillMount() {
    Image.getSize(this.props.uri, (width, height) => {
      if (this.props.width && !this.props.height) {
        this.setState({ width: this.props.width, height: height * (this.props.width / width) });
      } else if (!this.props.width && this.props.height) {
        this.setState({ width: width * (this.props.height / height), height: this.props.height });
      } else {
        this.setState({ width: width, height: height });
      }
    });
  }

  render() {
    return (
      <Image source={this.state.source} style={{ height: this.state.height, width: this.state.width }} />
    );
  }
}

ScaledImage.propTypes = propTypes;
```
上面这个组件，如果提供了宽高，图片也能按照指定宽高展示，如果只提供了宽或者高，那么图片就按照原图的比例，根据这个设定的宽或者高来等比例缩放展示，如果不提供宽或者高，则依据原图宽高展示。

- 方法二： Image 组件还提供了一个方法,预加载一个远程图片(将其下载到本地磁盘缓存)的方法，下载到本地缓存之后，就跟展示静态图片资源一样了。
```javascript
static prefetch(url: string) 
```

#### 参考链接
- [React Native 英文文档](https://facebook.github.io/react-native/docs/images.html)
- [React Native 中文文档](http://reactnative.cn/docs/0.47/images.html#content)
