### RxSwift散乱整理     
---
#### 1.RxSwift是什么  
**RxSwift**是应用于Swift语言的响应式编程框架。遵从`Rx`社区的响应式协议。
#### 2.RxSwift可以解决什么问题
- 整合苹果官方的异步编程API（NotificationCenter Delegate GCD Closure）
- 简化异步编程
- 利用函数式编程和响应式编程，解决命令式编程的固有问题
#### 3.RxSwift的好处
> 响应式编程提高了代码的抽象层级，所以你可以只关注定义了业务逻辑的那些相互依赖的事件，而非纠缠于大量的实现细节。RP 的代码往往会更加简明。


#### 0.待整理的杂项
> 在学习过程中最困难的一部分是 以响应式编程的方式思考     

[响应式编程（Reactive Programming）介绍](https://zhuanlan.zhihu.com/p/27678951)     

> The three building blocks of Rx code are **observables**, **operators**, and **schedulers**.     


**Observable<T>** 可以发送三种事件：`next`、`complete`、`error`。         
Sequences分为有限序列和无限序列。       
Subjects act as both an observable and an observer.     
Stream Sequence Signal Observable....       
Observer,subscribe,next,complete,error.Sheduler.Subject,  

==下面的类或者协议还有各种扩展==
### Observable      
```
public class Observable<Element> : ObservableType {
    /// ObservableType是protocol，实现类需要指定E的具体类型
    public typealias E = Element
    
    //ObserverType和ObservableType指定的E需要一致
    public func subscribe<O: ObserverType>(_ observer: O) -> Disposable where O.E == E {
        rxAbstractMethod()
    }
    
    public func asObservable() -> Observable<E> {
        return self
    }
}
```
### ObservableConvertibleType
```
public protocol ObservableConvertibleType {
    /// Type of elements in sequence.
    associatedtype E

    /// Converts `self` to `Observable` sequence.
    ///
    /// - returns: Observable sequence that represents `self`.
    func asObservable() -> Observable<E>
}
```
### ObservableType:ObservableConvertibleType
```
public protocol ObservableType : ObservableConvertibleType {
    func subscribe<O: ObserverType>(_ observer: O) -> Disposable where O.E == E
}
```
### ObserverType
```
public protocol ObserverType {
    /// The type of elements in sequence that observer can observe.
    associatedtype E

    /// Notify observer about sequence event.
    ///
    /// - parameter event: Event that occurred.
    func on(_ event: Event<E>)
}
```
### Event
```
public enum Event<Element> {
    /// Next element is produced.
    case next(Element)

    /// Sequence terminated with an error.
    case error(Swift.Error)

    /// Sequence completed successfully.
    case completed
}
```
### Disposable
```
/// Respresents a disposable resource.
public protocol Disposable {
    /// Dispose resource.
    func dispose()
}
```
### Producer:Observale

### 目录结构
```
.
├── AnyObserver.swift
├── Cancelable.swift
├── Concurrency
│   ├── AsyncLock.swift
│   ├── Lock.swift
│   ├── LockOwnerType.swift
│   ├── SynchronizedDisposeType.swift
│   ├── SynchronizedOnType.swift
│   └── SynchronizedUnsubscribeType.swift
├── ConnectableObservableType.swift
├── Deprecated.swift
├── Disposable.swift
├── Disposables
│   ├── AnonymousDisposable.swift
│   ├── BinaryDisposable.swift
│   ├── BooleanDisposable.swift
│   ├── CompositeDisposable.swift
│   ├── Disposables.swift
│   ├── DisposeBag.swift
│   ├── DisposeBase.swift
│   ├── NopDisposable.swift
│   ├── RefCountDisposable.swift
│   ├── ScheduledDisposable.swift
│   ├── SerialDisposable.swift
│   ├── SingleAssignmentDisposable.swift
│   └── SubscriptionDisposable.swift
├── Errors.swift
├── Event.swift
├── Extensions
│   ├── Bag+Rx.swift
│   └── String+Rx.swift
├── GroupedObservable.swift
├── ImmediateSchedulerType.swift
├── Info.plist
├── Observable.swift
├── ObservableConvertibleType.swift
├── ObservableType+Extensions.swift
├── ObservableType.swift
├── Observables
│   ├── AddRef.swift
│   ├── Amb.swift
│   ├── AsMaybe.swift
│   ├── AsSingle.swift
│   ├── Buffer.swift
│   ├── Catch.swift
│   ├── CombineLatest+Collection.swift
│   ├── CombineLatest+arity.swift
│   ├── CombineLatest+arity.tt
│   ├── CombineLatest.swift
│   ├── Concat.swift
│   ├── Create.swift
│   ├── Debounce.swift
│   ├── Debug.swift
│   ├── DefaultIfEmpty.swift
│   ├── Deferred.swift
│   ├── Delay.swift
│   ├── DelaySubscription.swift
│   ├── Dematerialize.swift
│   ├── DistinctUntilChanged.swift
│   ├── Do.swift
│   ├── ElementAt.swift
│   ├── Empty.swift
│   ├── Enumerated.swift
│   ├── Error.swift
│   ├── Filter.swift
│   ├── First.swift
│   ├── Generate.swift
│   ├── GroupBy.swift
│   ├── Just.swift
│   ├── Map.swift
│   ├── Materialize.swift
│   ├── Merge.swift
│   ├── Multicast.swift
│   ├── Never.swift
│   ├── ObserveOn.swift
│   ├── Optional.swift
│   ├── Producer.swift
│   ├── Range.swift
│   ├── Reduce.swift
│   ├── Repeat.swift
│   ├── RetryWhen.swift
│   ├── Sample.swift
│   ├── Scan.swift
│   ├── Sequence.swift
│   ├── ShareReplayScope.swift
│   ├── SingleAsync.swift
│   ├── Sink.swift
│   ├── Skip.swift
│   ├── SkipUntil.swift
│   ├── SkipWhile.swift
│   ├── StartWith.swift
│   ├── SubscribeOn.swift
│   ├── Switch.swift
│   ├── SwitchIfEmpty.swift
│   ├── Take.swift
│   ├── TakeLast.swift
│   ├── TakeUntil.swift
│   ├── TakeWhile.swift
│   ├── Throttle.swift
│   ├── Timeout.swift
│   ├── Timer.swift
│   ├── ToArray.swift
│   ├── Using.swift
│   ├── Window.swift
│   ├── WithLatestFrom.swift
│   ├── Zip+Collection.swift
│   ├── Zip+arity.swift
│   ├── Zip+arity.tt
│   └── Zip.swift
├── ObserverType.swift
├── Observers
│   ├── AnonymousObserver.swift
│   ├── ObserverBase.swift
│   └── TailRecursiveSink.swift
├── Platform
│   ├── DataStructures
│   │   ├── Bag.swift -> ../../../Platform/DataStructures/Bag.swift
│   │   ├── InfiniteSequence.swift -> ../../../Platform/DataStructures/InfiniteSequence.swift
│   │   ├── PriorityQueue.swift -> ../../../Platform/DataStructures/PriorityQueue.swift
│   │   └── Queue.swift -> ../../../Platform/DataStructures/Queue.swift
│   ├── DeprecationWarner.swift -> ../../Platform/DeprecationWarner.swift
│   ├── DispatchQueue+Extensions.swift -> ../../Platform/DispatchQueue+Extensions.swift
│   ├── Platform.Darwin.swift -> ../../Platform/Platform.Darwin.swift
│   ├── Platform.Linux.swift -> ../../Platform/Platform.Linux.swift
│   └── RecursiveLock.swift -> ../../Platform/RecursiveLock.swift
├── Reactive.swift
├── Rx.swift
├── RxMutableBox.swift
├── SchedulerType.swift
├── Schedulers
│   ├── ConcurrentDispatchQueueScheduler.swift
│   ├── ConcurrentMainScheduler.swift
│   ├── CurrentThreadScheduler.swift
│   ├── HistoricalScheduler.swift
│   ├── HistoricalSchedulerTimeConverter.swift
│   ├── Internal
│   │   ├── DispatchQueueConfiguration.swift
│   │   ├── InvocableScheduledItem.swift
│   │   ├── InvocableType.swift
│   │   ├── ScheduledItem.swift
│   │   └── ScheduledItemType.swift
│   ├── MainScheduler.swift
│   ├── OperationQueueScheduler.swift
│   ├── RecursiveScheduler.swift
│   ├── SchedulerServices+Emulation.swift
│   ├── SerialDispatchQueueScheduler.swift
│   ├── VirtualTimeConverterType.swift
│   └── VirtualTimeScheduler.swift
├── Subjects
│   ├── AsyncSubject.swift
│   ├── BehaviorSubject.swift
│   ├── PublishSubject.swift
│   ├── ReplaySubject.swift
│   └── SubjectType.swift
├── SwiftSupport
│   └── SwiftSupport.swift
└── Traits
    ├── Completable+AndThen.swift
    ├── Completable.swift
    ├── Maybe.swift
    ├── ObservableType+PrimitiveSequence.swift
    ├── PrimitiveSequence+Zip+arity.swift
    ├── PrimitiveSequence+Zip+arity.tt
    ├── PrimitiveSequence.swift
    └── Single.swift
```
### TIPS
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

### ObserveOn VS SubscribeOn
- SubscribeOn       
指定 ```let subscription = Observable.subscribe(self)``` 语句的执行线程。
- ObserveOn     
指定`Observer.on(event)`方法的执行线程