# STEP.0
公司网络使用`cocoapods` 安装`Crashlytics` `Fabric`时，偶尔会下载失败。

# 解决方法
- 将`Crashlytics` `Fabric`的`Framework`手动导入工程，脱离`pod`管理，他们本身也不是开源工程
    - 优点：不会再出现安装失败的现象
    - 缺点：升级时也需要手动
- 将代码托管到自己的`github`上，当成私有仓库
- 某次安装`Pods`成功，将其上传到`SVN`

# Gem是什么
>  Gem可以用来扩展或修改在Ruby应用程序功能。 通常他们用于分发可重用的功能,与其他ruby爱好者们用于共享他们的应用程序和库。      

简单讲`Gem`就是管理`Ruby`工程的第三方代码工具，类似
- js的npm
- Android的gradle
- Java的maven
- iOS的cocoapods

# cocoapods和Gem的关系
cocoapod是用ruby开发的，作为ruby的第三方库托管到Gem上，官方Gem地址被墙，目前国内提供的镜像地址为：`https://gems.ruby-china.com/`（2018-07-05）

# cocoapods管理的第三方库
- [官方托管地址](https://github.com/CocoaPods/Specs):可以在里面检索各种常用第三方库的描述文件，文件中包含了第三方库的下载地址。如果个人项目想要放到里面需要向官方注册。
- 也可以搭建个人[私有仓库](https://www.jianshu.com/p/0e1d796b2a42)

# cocoapods检索第三库过程
1. 先检测本地同步的repo-master
2. 再检测远程的repo-master
3. 检测私有仓库