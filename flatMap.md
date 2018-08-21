# flatMap

函数式编程中的`map` `reduce` `filter`算子很好理解。   
但是`flatMap`一直让我很困惑。   
今天阅读《Swift进阶》时，书中的例子让我恍然大悟。   
记录如下：
我想提取单个文本文件中的URL，有如下函数原型   
```swift
func extractLinks(from file: String) -> [URL]
```   
如果我有很多文本文件改如何处理呢？     
```swift
let fileArray: [String] = ...
let nestedLinks = fileArray.map(extractLinks)
```   
**但是**此时得到结果是嵌套数据    
```swift
[["http://","https://"],["smb://"]....,["ftp://"]]
```
整合成为一个数组， 
```swift
let links = nestedLinks.joined()
```
下面轮到**flatMap**登场：
```swift
fileArray.flatMap(extractLinks)
```   
flatMap与上面的map+joined等价。
**完**
