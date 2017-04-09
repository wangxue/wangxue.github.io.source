---
title: 提取android手机中apk文件
---

参考http://stackoverflow.com/a/11013175

1. 获取包名
> adb shell pm list packages
2. 获取包路径
> adb shell pm path 'package-name'
3. 获取包
> adb pull 'package-name' package-name.apk