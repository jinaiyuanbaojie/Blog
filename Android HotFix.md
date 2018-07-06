# 热修复流派
- Native: 利用JNI改变调用方法的入口
- Java：多dex打包，补丁dex插入前面
    - Qzone: 差量dex
    - Tinker: 全量dex(dexdiff算法本地合成)

# ClassLoader
- 加载Java代码到虚拟机
- 双亲委派模式找出Java类：先托管给父类查找，如果父类查找不到再自己查找，如此递归。
- Android平台下提供特定的`DexClassLoader`,会按顺序在dex数组中查找类，如果找到会忽略后面的相同类。

# Android编译方式
- JIT 及时编译：安装快，运行时速度会变慢
- AOT Ahead of time：安装包变大，安装时间变长，运行速度变快
- 混合编译：上述方式的综合

# Android虚拟机发展
- Dalvik：～Android4.0
- ART： Android5.0～
- ART beta: Android4.0
- ART: Android Run Time代替Dalvik的新虚拟机

# 编译方式与版本对应关系
- JIT：Dalvik虚拟机 ～Android4.0
- AOT：ART虚拟机 Android5.0～Android6.0
- 混合编译：Android7.0～

# 插桩CLASS_ISPREVERIFED
- 优化dex -> odex
- 满足条件的类会被打桩`CLASS_ISPREVERIFED`标记
- 被打桩的类引用热修复的类时会报错
- 打桩是为了优化Java代码的装载速度，当A类被标记，引用的B类不在原理的dex文件而在补丁dex文件中时，会报错