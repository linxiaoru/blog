---
title: react-native 项目报错解决办法收集
date: 2017-09-05 01:28:37
tags: ['React Native']
---
#### 文章目的
收集日常工作及学习过程中，项目报错的内容及解决办法。

#### 格式
**错误码或报错提示语句**
- 解决办法一：
- 解决办法二：
- ...

#### 错误收集
**1.npm install 安装错误，关键词： -4048 rename**
- 解决办法一：
	1. disabled anti-virus
	2. 有线连接，不用wifi
	3. npm cache clean
	4. rm node_modules
	5. npm install


**2.Error while uploading app-debug.apk : Unknown failure ([CDS]close[0]):app:installDebug FAILED**
- 解决办法一：Downgrade your gradle version（降低 gradle 版本）
 1. 在 `sampleProject/android/build.gradle` 将第八行修改为 ` com.android.tools.build:gradle:1.2.3 or 1.2.1 ` 
 
 2. 在 `sampleProject/android/gradle/wrapper/gradle-wrapper.properties` 将第五行修改为 `https://services.gradle.org/distributions/gradle-2.2-all.zip`



