+++
title = 'Concurrency in Swift'
date = 2024-01-07T00:00:00+03:00
tags = ["Swift", "Concurrency"]
draft = false
+++

### What is concurrency?
The system can perform multiple tasks simultaneously. By tasks, I mean `code or instructions`.
Modern computer chips have multiple cores that allow developers to create and run various tasks on multiple cores. Even if your chip has one core operating system it will provide context switching mechanism by enabling it to execute multiple tasks concurrently.

### Material about processes, threads
I will skip explaining concepts about processes and threads because it is a vast topic, and it will take a lot of time to explain it. I attached links to the material to help you understand it more deeply.
https://youtu.be/4rLW7zg21gI?si=49hq8Wrbpmeev41k
https://youtu.be/r2__Rw8vu1M?si=b7b257Qu4Bty7OxA
I will focus on implementation.

### The `old` and `modern` way of implementing concurrency
You can divide concurrency implementation into `old` or `unstructured` and `modern` or `structured`.

By `old`, I mean GCD (Grand Central Dispatch).
By `modern`, I mean async/await, actor, and Task.

In this article, I will talk about the `old` way.
GCD helps you keep your distance from manually managing threads and avoid unnecessary complexity, and it does it by providing API.
One of these APIs is DispatchQueue.

### DispatchQueue
By default, `DispatchQueue` is serial; all work on this queue will be executed sequentially.
`DispatchQueue` has access to the `main` property and the `global()` method.
The `main` property returns the serial queue associated with the main thread of the current process.
The `global()` method returns a concurrent queue specified by quality-of-service level.

``` swift
public class func global(qos: DispatchQoS.QoSClass = .default) -> DispatchQueue
```

You can pass many parameters when you try to initialize a new queue.
``` swift
 public convenience init(label: String, qos: DispatchQoS = .unspecified, attributes: DispatchQueue.Attributes = [], autoreleaseFrequency: DispatchQueue.AutoreleaseFrequency = .inherit, target: DispatchQueue? = nil)
 ``` 

Let’s talk about three of them (label, qos, attributes).
The first is `label,` which is used mainly for debugging and identification.

``` swift
let queue = DispatchQueue(label: "com.example.myqueue")
``` 

The second one is `qos` (Quality Of Service) allows you to choose the priority in which you like to run your task. You can choose between `background`, `utility`, `default`, `userInitiated`, `userIneractive`, and `unspecified` priorities.

``` swift
/// qos_class_t
public struct DispatchQoS : Equatable {
    public let qosClass: DispatchQoS.QoSClass
    public let relativePriority: Int
    @available(macOS 10.10, iOS 8.0, *)
    public static let background: DispatchQoS
    @available(macOS 10.10, iOS 8.0, *)
    public static let utility: DispatchQoS
    @available(macOS 10.10, iOS 8.0, *)
    public static let `default`: DispatchQoS
    @available(macOS 10.10, iOS 8.0, *)
    public static let userInitiated: DispatchQoS
    @available(macOS 10.10, iOS 8.0, *)
    public static let userInteractive: DispatchQoS
    public static let unspecified: DispatchQoS
}
``` 
`userIneractive` has the highest priority; it usually calls when you need to display UI almost immediately.

`background`, on the other hand, has the lowest priority.

### How to achieve concurrency with DispatchQueue API?
You can use a serial queue with the following:
`sync` functionality allows you to wait until the block you passed finishes its work.

``` swift
let serialQueue = DispatchQueue(label: "com.example.myqueue.serial")
serialQueue.sync {}
``` 
`async` functionality will `schedule` your work and be executed later in time.

``` swift
let serialQueue = DispatchQueue(label: "com.example.myqueue.serial")
serialQueue.async {}
``` 

Or you can use a concurrent queue with similar methods but running your task in parallel.

``` swift
let concurrentQueue = DispatchQueue(label: "com.example.myqueue.concurrent", attributes: .concurrent)
concurrentQueue.sync {}
concurrentQueue.async {}
``` 

### The difference between serial queue and concurrent queue
The difference between a serial and concurrent queue is that you should not wait until the concurrent operation finishes work in the `async` block.

``` swift
let concurrentQueue = DispatchQueue(label: "com.example.myqueue.concurrent", attributes: .concurrent)
concurrentQueue.sync {
    for i in 1...5 {
        print("Task \(i) is running on Concurrent Queue")
        sleep(1) // Simulate some work
    }
}
concurrentQueue.sync {
    for i in 6...10 {
        print("Task \(i) is running on Concurrent Queue")
        sleep(1) // Simulate some work
    }
}
// prints
Task 1 is running on Concurrent Queue
Task 6 is running on Concurrent Queue
Task 2 is running on Concurrent Queue
Task 7 is running on Concurrent Queue
Task 3 is running on Concurrent Queue
Task 8 is running on Concurrent Queue
Task 4 is running on Concurrent Queue
Task 9 is running on Concurrent Queue
Task 5 is running on Concurrent Queue
Task 10 is running on Concurrent Queue
``` 

The serial queue executes tasks in order, and you should wait until the first `async` block finishes its work to start the second block.

``` swift
let serialQueue = DispatchQueue(label: "com.example.myqueue.serual")
serialQueue.sync {
    for i in 1...5 {
        print("Task \(i) is running on Serial Queue")
        sleep(1) // Simulate some work
    }
}
serialQueue.sync {
    for i in 6...10 {
        print("Task \(i) is running on Serial Queue")
        sleep(1) // Simulate some work
    }
}

// prints
Task 1 is running on Serial Queue
Task 2 is running on Serial Queue
Task 3 is running on Serial Queue
Task 4 is running on Serial Queue
Task 5 is running on Serial Queue
Task 6 is running on Serial Queue
Task 7 is running on Serial Queue
Task 8 is running on Serial Queue
Task 9 is running on Serial Queue
Task 10 is running on Serial Queue
``` 

When you try to use the `sync` functionality, it behaves similarly on serial and concurrent queues by executing each task step by step and waiting till each block finishes its work.

``` swift
let serialQueue = DispatchQueue(label: "com.example.myqueue.serial")
serialQueue.sync {
    for i in 1...5 {
        print("Task \(i) is running on Serial Queue")
        sleep(1) // Simulate some work
    }
}
serialQueue.sync {
    for i in 6...10 {
        print("Task \(i) is running on Serial Queue")
        sleep(1) // Simulate some work
    }
}
// prints
Task 1 is running on Serial Queue
Task 2 is running on Serial Queue
Task 3 is running on Serial Queue
Task 4 is running on Serial Queue
Task 5 is running on Serial Queue
Task 6 is running on Serial Queue
Task 7 is running on Serial Queue
Task 8 is running on Serial Queue
Task 9 is running on Serial Queue
Task 10 is running on Serial Queue
``` 
``` swift
let concurrentQueue = DispatchQueue(label: "com.example.myqueue.concurrent", attributes: .concurrent)
concurrentQueue.sync {
    for i in 1...5 {
        print("Task \(i) is running on Concurrent Queue")
        sleep(1) // Simulate some work
    }
}
concurrentQueue.sync {
    for i in 6...10 {
        print("Task \(i) is running on Concurrent Queue")
        sleep(1) // Simulate some work
    }
}
// prints
Task 1 is running on Concurrent Queue
Task 2 is running on Concurrent Queue
Task 3 is running on Concurrent Queue
Task 4 is running on Concurrent Queue
Task 5 is running on Concurrent Queue
Task 6 is running on Concurrent Queue
Task 7 is running on Concurrent Queue
Task 8 is running on Concurrent Queue
Task 9 is running on Concurrent Queue
Task 10 is running on Concurrent Queue
``` 