---
title: 部署hexo博客
date: 2017-03-27 00:08:23
tags:
---

这个博客是由hexo生成的静态博客，使用indigo主题，部署在github page上，此处记录完整的部署过程。

### 安装node.js和git和hexo
此处可以参考hexo官方wiki，https://hexo.io/zh-cn/docs/index.html
### 生成博客并托管
参考https://hexo.io/zh-cn/docs/setup.html
需要注意的是由于我们可能在多台电脑操作，或者哪天本地hexo环境破坏，会比较麻烦，所以我将整个hexo project作为一个git库管理，并且托管到github上
```
cd <folder>
git init
git add . --all
git commit -m "init hexo"
git add remote origin git@github.com:wangxue.github.com.source
git push origin master
```
### 切换主题为indigo
参考https://github.com/yscoder/hexo-theme-indigo/wiki/%E5%AE%89%E8%A3%85
需要注意的是我们的hexo文件夹本身也是一个git仓库，然后这个themes文件夹中的indigo只能作为一个git submodule
那么在下载indigo主题时，需要采用git submodule的方式来操作
```
git add submodule git@github.com:yscoder/hexo-theme-indigo.git themes/indigo themes/indigo
```
将来我们在其他电脑上下载自己的博客代码后，同时也需要更新子模块
```
git clone git@github.com:wangxue.github.io.source hexo-source
cd hexo-source
git submodule update --init --recursive
```
### 部署静态网页
参考https://hexo.io/zh-cn/docs/
### 遇到的问题
1. ssh -t git@github.com时显示permission denied
[私钥文件名不对导致](http://blog.csdn.net/sunnypotter/article/details/18948053/)
2. 如何删除文章：
执行hexo clean，在_post中删除文章，执行hexo -g d重新部署
3. 编译时报错
报错如下
```
04:50:25.815 FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Template render error: (unknown path) [Line 7, Column 23]
  Error: Unable to call `the return value of (posts["first"])["updated"]["toISOString"]`, which is undefined or falsey
    at Object.exports.prettifyError (C:\source\github\wangxue.github.io.source\node_modules\nunjucks\src\lib.js:34:15)
    at C:\source\github\wangxue.github.io.source\node_modules\nunjucks\src\environment.js:486:31
    at new_cls.root [as rootRenderFunc] (eval at _compile (C:\source\github\wangxue.github.io.source\node_modules\nunjucks\src\environment.js:565:24), <anonymous>:161:3)
    at new_cls.render (C:\source\github\wangxue.github.io.source\node_modules\nunjucks\src\environment.js:479:15)
    at Hexo.module.exports (C:\source\github\wangxue.github.io.source\node_modules\hexo-generator-feed\lib\generator.js:28:22)
    at Hexo.tryCatcher (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\util.js:16:23)
    at Hexo.<anonymous> (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\method.js:15:34)
    at C:\source\github\wangxue.github.io.source\node_modules\hexo\lib\hexo\index.js:337:24
    at tryCatcher (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\util.js:16:23)
    at MappingPromiseArray._promiseFulfilled (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\map.js:61:38)
    at MappingPromiseArray.PromiseArray._iterate (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\promise_array.js:114:31)
    at MappingPromiseArray.init (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\promise_array.js:78:10)
    at MappingPromiseArray._asyncInit (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\map.js:30:10)
    at Async._drainQueue (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\async.js:138:12)
    at Async._drainQueues (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\async.js:143:10)
    at Immediate.Async.drainQueues (C:\source\github\wangxue.github.io.source\node_modules\bluebird\js\release\async.js:17:14)
    at runCallback (timers.js:666:20)
    at tryOnImmediate (timers.js:639:5)
    at processImmediate [as _immediateCallback] (timers.js:611:5)
```
搜索后发现暂时无解https://github.com/hexojs/hexo-generator-feed/issues/43
屏蔽该插件即可，注意不是按照网友说的在_config.yml中屏蔽，而是直接卸载该模块，之后可以通过执行hexo config查看feed配置项是否还在
```
npm uninstall hexo-generator-feed --save
```
需要注意的是在安装hexo的插件时不要用-g参数，而是使用hexo.io及各主题教程中的安装命令，安装的模块位于hexo项目内，hexo generate时才可以引用到，否则会出现异常。