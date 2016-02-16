# IntelliJ IDEA系列 创建一个项目

`shift + command + F9`  compile one class.

## 修改主题

IntelliJ IDEA对目录做了调整，把偏好设置调整到了如下目录：

IntelliJ IDEA --> preferences(快捷键：⌘,) --> Appearance & Behavior --> Appearance --> UI Options --> Theme

旧版本的可以在`file --> Settings中找到`。

## 编译
有如下三种编译：

* Compile

对特定的代码文件进行强制编译；

* Rebuild

对选择的project进行强制编译；

* Make

对选定的Project或者Module进行编译，只编译改动过的文件；

### Run配置
Run --> Edit Configurations


## 项目结构设置
file --> project structrue(快捷键：⌘;)
可以在这里管理SDK版本(Platform Settings --> SDKs)，设置项目SDK(Project Settings --> Project)。

### language level
对于大型项目，各个 Module 用到的 SDK 和 language level 很有可能是各不一样的，IntelliJ IDEA 对此也进行了支持。(Project Settings --> Modules --> Sources --> Lauguage Level...)

## 编辑
### 实时代码模板
```
IntelliJ IDEA --> Editor --> Live Template
```

提示快捷键：`Tab` 和 `Ctrl + J`

### 文件代码模板

## References

https://github.com/judasn/IntelliJ-IDEA-Tutorial

[IntelliJ IDEA使用教程](http://wiki.jikexueyuan.com/project/intellij-idea-tutorial/)



