在使用spm之前，首先得安装nodejs，因为spm是nodejs的一个package。[使用spm@3构建seajs项目](http://qianduanblog.com/post/js-learning-42-using-spm-3-build-seajs-project.html "使用spm@3构建seajs项目")

[spm 3官方教程](http://spmjs.io/documentation "spm 3官方教程")

构建目录：
```
dist  构建目标文件夹
examples  相关使用文档
spm_modules   依赖的js文件仓库，spm install的时候下载到里面
tests  测试案例
xx.js  
package.json  spm维护的依赖配置文件，用于生产dist目录的相关配置
```

[expectjs](http://spmjs.io/package/expect.js "expectjs")

[为什么 SeaJS 模块的合并这么麻烦](http://chaoskeh.com/blog/why-its-hard-to-combo-seajs-modules.html "为什么 SeaJS 模块的合并这么麻烦")

###安装nodejs

[seajs](http://seajs.org/docs/#docs "seajs")

配置NODE_PATH

[nodejs 中的 NODE_PATH](http://segmentfault.com/blog/yinchangsheng/1190000002478924 "nodejs 中的 NODE_PATH")


windows下需要安装[make](http://gnuwin32.sourceforge.net/packages/make.htm "make")

下载[demo](https://github.com/seajs/examples "demo")编译执行






SeaJS遵循[CMD](https://github.com/cmdjs/specification/blob/master/draft/module.md "CMD")规范，一个模块就是一个文件，通过`require` 关键词引入外部模块，每次加载需要直接使用到的模块的时候，模块内部 `require` 进来的js也会一并加载，为了让SeaJS在加载之前把所有相关联的JS都能够通过一个JS文件一次性加载进来，就需要用到SeaJS的[Transport](http://www.zhihu.com/question/20789867/answer/16187950 "Transport")格式
没转换前Modules/Wrappingsgui规范格式如下：
```
define(function(require, exports, module) {
   var a = require("a")
   exports.foo = ...
})
```

CMD模块在正式上线前，通过构建工具（如SPM）先转换为Modules/Transport格式：

```
define("id", ["dep-1", "dep-2"], function(require, exports, module) {
   // source code
})
```

进一步通过构建工具，就可以一并吧 `require` 引入进来的模块一起压缩合并到同一个文件中了。




##说明
简单来说，通过使用SeaJS，我们可以把JS代码模块化，在上线之前，通过spm这样的构建工具，把自己开发的模块压缩合并，当成SeaJS的一个模块来使用。

##Step by step

下面一步一步来实现这个过程：


##正面项目接口

整体目录结构如下：
```
project   // 项目跟目录 
|
|-assets  // web资源文件
  |
  |- sea-modules  // SeaJS相关模块
  |  
  |- static  // 模块开发路径
  |   |
  |   |- first-module
  |       |- dist  // 我的模块构建目标文件夹
  |       |- src  // 我的模块开发文件夹
  |       |- package.json  // spm编译配置
  |
  |- app  // 我的模块使用例子
  |
```

##创建目录结构

###step1、创建项目 project

###step2、创建web资源文件夹
在project里面新建一个assetsweb资源文件夹，专门用来存放JS，CSS等资源，我们的模块开发就在这个目录下面进行

###step3、在assets下面创建sea-modules文件夹
这个文件夹是用来存放SeaJS模块的，我们需要用到的模块，包括自己实现的模块最终都要存放到这里

###step4、在assets目录下创建 static 文件夹
这个文件夹用于模块开发

###step5、在 static 目录下创建 first-module 文件夹
这个文件夹用于存储我的第一个模块的开发代码

###step6、在 first-module 目录下创建 src 目录，这个目录用于存放我们模块开发的代码

###step7、在assets下面创建 app 文件夹
这个文件夹存放我们演示调用模块代码的HTML文件

##使用spm初始化构建

###首先你需要通过npm安装spm，而npm是Node.js里面的一个工具，所以你需要先这样搭建环境：

* 安装[Node.js](http://nodejs.org/ "Node.js")
* 安装spm    
目前最新的是3版本的，建议安装最新的，详细可以参考官方文档：
[http://spmjs.io/](http://spmjs.io/documentation/getting-started "spmjs.io")    
这里只要在cmd里面执行以下一句就可以了
     
```
$ npm install spm -g
```

* 使用spm初始化项目目录
    
在first-module目录下执行

```
spm init
```

接下来按照提示分表填写 Package name, Version, Description, Author即可。

执行完之后生成如下目录：
![]()

接下来就可以开始开发自己的模块了。

##模块开发

###编辑我们自己的模块代码
在src目录下创建我们的模块代码 index.js


http://blog.csdn.net/huyoo/article/details/21703701
