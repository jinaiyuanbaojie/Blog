[TOC]

# Any AnyObject AnyClass  
> 三者的含义，顾名思义
- Any: 任何变量, struct enum function
- AnyObject: 任何class的实例，与ObjC中的id等价
- AnyClass：任何对象的类对象,MetaType元类型。与ObjC中[NSObject class]类似。UIView.self <==> [UIView class]  
[blog](http://swifter.tips/self-anyclass/)

# nil Nil NULL NSNull
- nil 指向不存在对象的指针
- Nil 指向不存在类的指针 Class clazz = Nil
- NULL 兼容C
- NSNull 对象，表示“空”
> [nshipster](https://nshipster.cn/nil/)

# id NSObject `id<NSObject>`
- id: 任何对象，Objc中NSObject并非所有类的父类，比如NSProxy
- NSObject: 父类为NSObject的对象
- `id<NSObject>`: 任何实现了NSObject协议的类的对象  
> ObjC中既有名为NSObject的protocol也有名为NSObject的class 

# id instancetype
> (id) init vs (instancetype) init
instancetype返回当前类型，辅助编译器提供更多信息


# Lifecycle of UIViewController
```swift
initWithNibName:bundle:
loadView
viewDidLoad
viewWillAppear
loadViewIfNeeded
viewWillLayoutSubviews
viewDidLayoutSubviews
viewDidAppear
viewWillDisappear
viewDidDisappear
didReceiveMemoryWarning
dealloc
```   
> initWithNibName:bundle: 初始化方法两个参数都可以为空，系统会安装某种规则自动查找  
[UIViewController init]方法也会调用initWithNibName:bundle:  
调用initWithNibName:bundle:, 就不能override  loadView方法，否则会nib会被覆盖，导致失效。

# Java调用爷爷类方法
- super.super 不存在的
> [stackoverflow](https://stackoverflow.com/questions/586363/why-is-super-super-method-not-allowed-in-java)

# C++ 调用爷爷类方法
> Grandfather::method()

# nib xib
- xib是nib的进化版本 nib->nib2.0->nib3.0->xib
- xib是xml的文本文件，nib是二进制文件
- xib也会被编译器编译，会被编译成nib文件  
> [stackoverflow](https://stackoverflow.com/questions/3726400/what-is-the-difference-between-nib-and-xib-interface-builder-file-formats)  
[xib vs storyboard vs code](https://onevcat.com/2013/12/code-vs-xib-vs-storyboard/)  

# ObjC super VS self
`[super method]`和`[self method]`对象的接受者都是`self`.
只是`super`告诉runtime从`self`的基类开始查找方法

# ObjC 2.0 内存布局 
- 1.0 ABI不兼容  
- 2.0 系统库添加字段不影响业务代码。ABI兼容  
- 大部分blog讲解的都是1.0的内存布局

[blog](http://quotation.github.io/objc/2015/05/21/objc-runtime-ivar-access.html) 

# ObjC category为何不能添加属性
此处**属性**应该是**成员**。  
@property = ivar + getter + setter  
- property 可以添加，通过编译。没意义，因为只有getter和setter，没有ivar
- ivar 不可以添加：原因对象在内存中的布局必须是固定大小的。否则运行时会出现问题：已经分配的对象与新分配的对象大小不一样，调用方法可能会导致crash  

solution: 添加关联对象  
[blog](http://quotation.github.io/objc/2015/05/21/objc-runtime-ivar-access.html)  
[Apple](https://developer.apple.com/documentation/objectivec/objective-c_runtime)

# 苹果开源代码
下载链接 http://opensource.apple.com/  
- runtime
- GCD
- CoreFoundation
- etc...

# module.modulemap
http://andelf.github.io/blog/2014/06/19/modules-for-swift/

# Codable原理  
[blog](https://techblog.toutiao.com/2017/07/05/session212/)

# Assets.car
> 编译后的ipa安装包，内部含有此文件。此文件是`image.xcassets`下图片资源的集合。

# main.bundle & sub.bundle
> 工程的所有资源文件：图片，文件，xib等等都会存放到手机的bundle路径下。  
根目录就是main.bundle, main.bundle下存在各种sub.bundle.例如AliSDK.bundle

# TIPS
- .tt文件 生成代码的模版文件 T4(Text Template Transformation Toolkit)
- `OSAtomicCompareAndSwap32Barrier(_oldValue _newValue _theValue)->Bool` 如果_oldValue == _theValue，那么设置_newValue并返回true。保证操作的原子性。
- `extension NSObject: ReactiveCompatible { }`
- `@inline(never)` 永远不要把函数编译成内联形式
- `@inline(__always)` 与上面相反
- **ContiguousArray** 不和OC桥接的话，比使用Array更加有效率
- **precondition(_ condition @escape)** 
- `swift(>=4.0)` 内置宏判断swift版本
- __attribute__((constructor)) main函数前执行某方法
- `.scpt`AppleScript是用在MacOSX上的脚本语言
- `insert_dylib`可以给App注入动态库。
- [在OC中使用 class 属性修饰符] 就是给类添加静态成员，但是不会自动合成getter setter，这么做是为了与Swift桥接。（早他妈该有这个东西了）
- NSURLCache
- @discardableResult 忽略不使用方法返回值的警告
