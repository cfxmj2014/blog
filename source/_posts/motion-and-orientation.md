---
title: iOS12.2+设备屏幕方向与运动失效问题
date: 2019-05-27 18:12:04
tags: 
  - 日常开发总结
  - Web
categories: 
  - 日常开发总结
  - Web
---

### - 介绍
日常开发中，如果使用加速度计、陀螺仪和罗盘相关功能，需要监听设备方向事件`deviceorientation`和设备运动事件`devicemotion`。

[相关文档](https://developers.google.com/web/fundamentals/native-hardware/device-orientation/)

<!-- more -->

### - 坑
API用法很简单，但是在前一阵实际开发时，在某些苹果手机上总是监听不到这两个事件，后来查到在`ios12.2+`以上，由于安全限制，必须在`https`协议下才能监听，而且在`Safari`下，默认会关闭设备监听事件，需要主动开启。

设置：`Settings > Safari > Privacy & Security > Motion & Orientation Access`

[相关文档](https://www.macrumors.com/2019/02/04/ios-12-2-safari-motion-orientation-access-toggle/)

### - 注意事项
在处理完业务逻辑，记得移除设备监听事件`removeEventListener`

### - 推荐
[What Web Can Do Today](https://whatwebcando.today/device-motion.html)