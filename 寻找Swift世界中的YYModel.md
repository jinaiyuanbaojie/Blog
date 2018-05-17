# 寻找Swift世界中的YYModel

## 怀念YYModel  
Objective-C世界中的json序列化第一神器我认为非[`YYModel`](https://github.com/ibireme/YYModel)莫属：  
- 代码侵入性极小，不要求Model类继承某个基类或者实现某种协议。
- 效率极高，缓存反射信息，尽量使用C级别的API
- 安全，处理各种异常，基本做到不会因为服务器的脏数据导致app crash     

YYModel使用样例：   
```
// JSON:
{
    "uid":123456,
    "name":"Harry",
    "created":"1965-07-31T00:00:00+0000"
}

// Model:
@interface User : NSObject
@property UInt64 uid;
@property NSString *name;
@property NSDate *created;
@end
@implementation User
@end

	
// Convert json to model:
User *user = [User yy_modelWithJSON:json];
	
// Convert model to json:
NSDictionary *json = [user yy_modelToJSONObject];
```

## Swift世界中的YYModel
1. 原生的json转换方式
```
let jsonObject : AnyObject! = NSJSONSerialization.JSONObjectWithData(dataFromTwitter, options: NSJSONReadingOptions.MutableContainers, error: nil)
if let statusesArray = jsonObject as? NSArray{
    if let aStatus = statusesArray[0] as? NSDictionary{
        if let user = aStatus["user"] as? NSDictionary{
            if let userName = user["name"] as? NSDictionary{
                //好累，我就取个数据写了这么多。
            }
        }
    }
}
```
或者利用`optional`
```
let jsonObject : AnyObject! = NSJSONSerialization.JSONObjectWithData(dataFromTwitter, options: NSJSONReadingOptions.MutableContainers, error: nil)
if let userName = (((jsonObject as? NSArray)?[0] as? NSDictionary)?["user"] as? NSDictionary)?["name"]{
  //这个代码基本看不懂了啊！
}
```
以上[引用](http://tangplin.github.io/swiftyjson/)，显然原生的解析是让人抓狂的。
2. SwiftyJson解析
```
let json = JSON(data: dataFromNetworking)
if let userName = json[0]["user"]["name"].string{
  //就这么简单取到了。
}
```
相比原生的解析简单许多，但是仍然不如`YYModel`优雅。在同事的推荐下，发现了新大陆阿里巴巴的[`HandyJSON`](https://github.com/alibaba/HandyJSON/blob/master/README_cn.md)
3. HandyJSON解析        
解析样例
```
class BasicTypes: HandyJSON {
    var int: Int = 2
    var doubleOptional: Double?
    var stringImplicitlyUnwrapped: String!

    required init() {}
}

let jsonString = "{\"doubleOptional\":1.1,\"stringImplicitlyUnwrapped\":\"hello\",\"int\":1}"
if let object = BasicTypes.deserialize(from: jsonString) {
    print(object.int)
    print(object.doubleOptional!)
    print(object.stringImplicitlyUnwrapped)
}
```
很优雅的解决方案，但是官方文档下的这句话让人望而却步: 
>HandyJSON目前依赖于从Swift Runtime源码中推断的内存规则，任何变动我们将随时跟进。   

这是典型的给自己埋雷。
4. Codable  
[Codable](https://www.jianshu.com/p/bdd9c012df15)苹果官方支持，有保证。
# HandyJSON原理     
[TOC]