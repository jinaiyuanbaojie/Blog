# RunLoop初探

### RunLoop是什么
**一种do while模型**；      
**赋予与其关联线程源源不断处理事务的能力**;     
用伪代码表示如下：    

```
function loop() {
    initialize();
    do {
        var message = get_next_message();
        process_message(message);
    } while (message != quit);
}

作者：testman00
链接：http://www.jianshu.com/p/98f3f9f1d171
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
如果不使用RunLoop启动如下程序

```
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        NSLog(@"Hello world.");
    }
    return 0;
}
```
输出：

```
2017-09-22 09:19:29.380546 Client[37342:3565338] Hello world.
Program ended with exit code: 0
```
进程执行完任务，自动退出，无法做到源源不断的处理任务。

如果使用RunLoop启动如下程序

```
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        NSLog(@"Hello world.");
        CFRunLoopRef runLoop = CFRunLoopGetCurrent();
        NSLog(@"RunLoop will run.");
        CFRunLoopRun();
        NSLog(@"Program will exit.");
    }
    return 0;
}
```
输出：

```
2017-09-22 09:24:43.387610 Client[37488:3567968] Hello world.
2017-09-22 09:24:43.390165 Client[37488:3567968] RunLoop will run.
```
可见进程并未退出，而是在等待消息，处理任务。

**伪代码存在的问题**：过度消耗CUP资源。RunLoop不存在此问题，可以阻塞当前线程（**休眠**），可以唤醒当前线程。
  

### RunLoop内部数据结构
![image](https://ws3.sinaimg.cn/large/006tNc79ly1fjs3i5ckmfj306u059gm0.jpg)

**Mode**可以理解为RunLoop中不同的通道，RunLoop每次循环只处理一个通道中的任务。    
**Source**    可以认为是系统发送的事件，如点击事件。
> CFRunLoopSourceRef是事件产生的地方。Source有两个版本：Source0 和 Source1。

**Observer**可以自己注册监听RunLoop的生命周期。
示例代码：
```
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        NSLog(@"Hello world.");
        CFRunLoopRef runLoop = CFRunLoopGetCurrent();
        NSLog(@"RunLoop will run.");
        
        CFRunLoopObserverRef observer =
        CFRunLoopObserverCreateWithHandler(kCFAllocatorDefault,
                                           kCFRunLoopExit, true, 0,
                                           ^(CFRunLoopObserverRef observer, CFRunLoopActivity activity) {
            NSLog(@"wanna runloop exit...");
        });
        CFRunLoopAddObserver(runLoop, observer, kCFRunLoopDefaultMode);
        
        dispatch_queue_t queue = dispatch_queue_create("DTAsyncFileDeleterRemoveQueue", 0);
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), queue, ^{
            CFRunLoopStop(runLoop); //exit
        });
        
        CFRunLoopRun();
        NSLog(@"Program will exit.");
    }
    return 0;
}
```
输出：

```
2017-09-22 09:52:35.377697 Client[38198:3580524] Hello world.
2017-09-22 09:52:35.378163 Client[38198:3580524] RunLoop will run.
2017-09-22 09:52:37.545909 Client[38198:3580524] wanna runloop exit...
2017-09-22 09:52:37.545952 Client[38198:3580524] Program will exit.
Program ended with exit code: 0
```

**Timer**就是我们开发中用到的定时器。    

> 在 CoreFoundation 里面关于 RunLoop 有5个类:
> CFRunLoopRef
> CFRunLoopModeRef
> CFRunLoopSourceRef
> CFRunLoopTimerRef
> CFRunLoopObserverRef

### RunLoop的API以及源码
> CFRunLoop位于CoreFoundation层，是开源的。
- 创建RunLoop

```
/// 全局的Dictionary，key 是 pthread_t， value 是 CFRunLoopRef
/** 全局字典, 线程对应Runloop,如果没有值表示是第一次运行,创建主线程的RunLoop */
static CFMutableDictionaryRef loopsDic;
/// 访问 loopsDic 时的锁
static CFSpinLock_t loopsLock;

/// 获取一个 pthread 对应的 RunLoop。
/** get的时候内部自动创建一个runloop, 难怪currentRunLoop时就有runloop了 */
CFRunLoopRef _CFRunLoopGet(pthread_t thread) {
    OSSpinLockLock(&loopsLock);

    if (!loopsDic) {
        // 第一次进入时，初始化全局Dic，并先为主线程创建一个 RunLoop。
        loopsDic = CFDictionaryCreateMutable();
        CFRunLoopRef mainLoop = _CFRunLoopCreate();
        CFDictionarySetValue(loopsDic, pthread_main_thread_np(), mainLoop);
    }

    /// 直接从 Dictionary 里获取。
    CFRunLoopRef loop = CFDictionaryGetValue(loopsDic, thread));

    if (!loop) {
        /// 取不到时，创建一个
        loop = _CFRunLoopCreate();
        CFDictionarySetValue(loopsDic, thread, loop);
        /// 注册一个回调，当线程销毁时，顺便也销毁其对应的 RunLoop。
        _CFSetTSD(..., thread, loop, __CFFinalizeRunLoop);
    }

    OSSpinLockUnLock(&loopsLock);
    return loop;
}

/** 封装 获取 主线程 RunLoop方法 */
CFRunLoopRef CFRunLoopGetMain() {
    return _CFRunLoopGet(pthread_main_thread_np());
}

/** 封装 获取 当前线程 RunLoop方法 */
CFRunLoopRef CFRunLoopGetCurrent() {
    return _CFRunLoopGet(pthread_self());
}

作者：testman00
链接：http://www.jianshu.com/p/98f3f9f1d171
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

- 启动RunLoop
![image](https://ws4.sinaimg.cn/large/006tNc79ly1fjs6vnna4cj30bl08tdi1.jpg)
- 唤醒RunLoop

```
GCD's main queue is a serial queue. So, it can only run a single task at a time. Even if that task runs an inner run loop — for example, runs a modal dialog — then other tasks submitted to the main queue can't run until that has completed.

Tasks submitted using CFRunLoopPerformBlock() can run whenever the run loop is run in one of the target modes. That includes if the run loop is run from within a task that was submitted using CFRunLoopPerformBlock().

Consider the following examples:

CFRunLoopPerformBlock(CFRunLoopGetMain(), kCFRunLoopCommonModes, ^{
    printf("outer task milestone 1\n");
    CFRunLoopPerformBlock(CFRunLoopGetMain(), kCFRunLoopCommonModes, ^{
        printf("inner task\n");
    });
    [[NSRunLoop currentRunLoop] runUntilDate:[NSDate dateWithTimeIntervalSinceNow:1]];
    printf("outer task milestone 2\n");
});
produces output like:

outer task milestone 1
inner task
outer task milestone 2
While this:

dispatch_async(dispatch_get_main_queue(), ^{
    printf("outer task milestone 1\n");
    dispatch_async(dispatch_get_main_queue(), ^{
        printf("inner task\n");
    });
    [[NSRunLoop currentRunLoop] runUntilDate:[NSDate dateWithTimeIntervalSinceNow:1]];
    printf("outer task milestone 2\n");
});
produces:

outer task milestone 1
outer task milestone 2
inner task
```

- 停止RunLoop

```
#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        NSLog(@"Hello world.");
        CFRunLoopRef runLoop = CFRunLoopGetCurrent();
        
        dispatch_queue_t queue = dispatch_queue_create("DTAsyncFileDeleterRemoveQueue", 0);
        dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), queue, ^{
            //不会唤醒线程
            CFRunLoopPerformBlock(runLoop, kCFRunLoopDefaultMode, ^{
                NSLog(@"looper run0...");
                CFRunLoopStop(runLoop); //exit
            });
            CFRunLoopWakeUp(runLoop);
        });
        
        CFRunLoopRun();
    }
    return 0;
}
```
 输出
 
```
2017-09-22 13:36:58.345185 Client[42518:3636408] Hello world.
2017-09-22 13:37:00.399378 Client[42518:3636408] looper run0...
Program ended with exit code: 0
```


- 发送消息给RunLoop
  - timer
  - observer
  - source
  - block

### RunLoop Thread dispatch_queue 之间的关系
> dispatch_queue通过RunLoop向线程发送消息。消息已block的形式包装。
### RunLoop 使用场景
- CGD
- NSAtuoReleasePool
- UI绘制
- 点击事件

### 参考资料
1. [stackoverflow](https://stackoverflow.com/questions/12871737/cfrunloopperformblock-vs-dispatch-async/23819286#23819286) https://stackoverflow.com/questions/12871737/cfrunloopperformblock-vs-dispatch-async/23819286#23819286      
1. [简书](http://www.jianshu.com/p/98f3f9f1d171)    

2. 苹果官方文档 
3. Android:**Looper**


[TOC]