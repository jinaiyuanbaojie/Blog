## 问题：    
今天配合搭建新工程的CI构建环境。
构建脚本执行到`xcodebuild clean`时，`jekins`卡住日志一直loading卡住。
构建脚本与之前工程的完全一致。
旧工程可以正常构建。

## 原因：    
新工程使用了`CocoaPods`管理第三方代码。

## 解决方法：    
构建脚本需要额外添加指令，打开一下工程：
> open $projectPath/$projectname.xcworkspace
打开工程后，会自动生成一些额外cocoapods相关的文件，问题解决。
