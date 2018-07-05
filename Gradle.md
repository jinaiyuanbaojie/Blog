[TOC]

# Gradle
- Java的打包工具：Server或Android都可以
- 基于Groovy语言，Groovy语言运行在JVM上，会编译为class文件，所以可以与Java兼容。
- 可以管理Android各模块依赖关系

# Android工程结构
![](http://upload-images.jianshu.io/upload_images/2897814-cf10a6dfd2ff930a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)      

- setting.gradle 配置那些module参与编译构建
```
include ':MyApp', //main module
        ':subMoudle',
        ':Base:Utils1', //文件夹Base下的module Utils1
        ':Base:Utils2'
```

- 顶层build.gradle 各个module共享
```
//构建脚本（groovy语言）依赖的第三放插件和插件的代码仓库
buildscript {
    repositories {
        google()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'
    }
}

//Android项目（Java语言）依赖的第三放插件和插件的代码仓库
allprojects {
    repositories {
        google()
    }
}
```
- app下build.gradle 具体模块的构建指令
```
//编译MainModule所依赖的子模块以及jar包的检索路径
dependencies {
    api fileTree(include: ['*.jar'], dir: 'libs')
    api project(':subModule1')
    api project(':subModule2')
}
```
# 自定义groovy插件
- 直接在build.gradle中写逻辑：快速验证功能，复用性差
- 作为groovy lib被build.gradle引用
    - 新建Android Module/Android Library
    - 除build.gradle文件外的其余文件全都删除
    - 新建文件夹`src/main/groovy`目录
    - 配置`build.gradle`
    ```
    apply plugin: 'groovy' //必选
    apply plugin: 'maven'
    dependencies {
        //依赖的jar包或者gradle插件
    }
    ```
    - 新建resources/META-INF/gradle-plugins/`plugin.properties`
    - 在文件中配置自定义插件的入口
    
    `implementation-class=mypackage.MyPlugin`
    - 实现`MyPlugin`
    ```
    class MyPlugin implements Plugin<Project> {
        void apply(Project project) {
            //自定义Task也可以引用其他groovy类的方法
            project.task('myTask') << {
                println "Hello world"
            }
        }
    }
    ```
    - 命令行或者IDE右侧Task列表执行：gradlew myTask