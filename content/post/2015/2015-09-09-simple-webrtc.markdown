---
layout: post
title: "Simple WebRTC"
date: 2015-09-09T17:10:18+08:00
categories:
- 技术文章
tags: [Javascript, WebRTC]
---

### Step1
1. html->body中添加`<video />`，在JS中用`document.querySelector("video");`可以获取到该标签
2. JS中添加如下代码既可以操作视频流

```javascript
var constrains = {video:  {mandatory: {
      maxWidth: 640,
      maxHeight: 360 
}}};

function successCallback(localMediaStream) {
    window.stream = localMediaStream;
    var video = document.querySelector("video");
    video.src = window.URL.createObjectURL(localMediaStream);
    video.play();
}

function errorCallback(error) {
    console.log("navigator.getUserMedia error: ", error);
}

getUserMedia(constrains, successCallback, errorCallback);
```
`window.stream = localMediaStream;` 将stream导出到**window**中，可以省略

这一步只是设置本地视频的基本步骤，显示如何使用本地摄像头，还不牵涉到端对端的交互。
