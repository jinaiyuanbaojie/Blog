# XCConfig
`xcconfig`文件是Xcode的工程构建配置文件，相当于把Project->Build Settings中的各种选项**文本化**。   
你可以选中Build Settings中任何配置项，`cmd+c`，然后在`xcconfig`文件中`cmd+v`后就可以看到效果。

## Objective-C时代
因为Objective-C兼容C，所以也理所应当支持宏。    
在xcconfig文件中预定义一些宏，可以让代码中少很多`if{...}else{...}`的语句。类似于开关。

## Swift时代
Swift本身不再支持宏，所以OC时代的便利性下降。当然不支持宏是正确的。可是如何在Swift中享受OC时代的便利呢？   
1. 桥接OC
2. 只能设置Swift支持的Flag （**Other Swift Flags**）
