+++
title = 'Modern Concurrency'
date = 2024-02-04T08:29:30+03:00
tags = ["Modern Concurrency", "Concurrency", "Swift"]
draft = false
+++

### When was it introduced?
It was introduced in [Swift 5.5](https://github.com/apple/swift/tree/swift-5.5-RELEASE) at [WWDC 2021](https://developer.apple.com/videos/wwdc2021/).

You can find the more comprehensive info about Modern Concurrency in [Swift Concurrency Manifesto](https://gist.github.com/lattner/31ed37682ef1576b16bca1432ea9f782#swift-concurrency-manifesto).

### What are actors?
[Actors](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Actors) eliminate shared mutable state and explicit synchronization through deep copying of all the data that passed to an actor to a message sent and preventing direct access to actor state. Actors are [reference types](https://www.swift.org/documentation/articles/value-and-reference-types.html#:~:text=changing%20the%20value.-,Reference%20Types,-In%20Swift%2C%20classes).

``` swift 
actor DatabaseManager {
    private var data: [String: String] = [:]

    func readData(key: String) -> String? {
        data[key]
    }

    func writeData(key: String, value: String) {
        data[key] = value
    }
}
```

### What is an asynchronous function?
[The asynchronous function](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Defining-and-Calling-Asynchronous-Functions) or asynchronous method can be suspended while it is partway through execution. It can pause in the middle when it’s waiting for something.

``` swift 
func someAsyncOperation(index: Int) async throws -> String {
    try await Task.sleep(nanoseconds: 1_000_000_000)
    return "Operation \(index) Completed"
}

func performAsyncOperations() async throws {
    for index in 1...1000 {
        Task.detached {
            print("Start of operation \(index)")
            let result = try await someAsyncOperation(index: index)
            print("End of operation \(index) with result: \(result)")
            return result
        }
    }
}

Task {
    print("Start of example")
    try await performAsyncOperationsWithYield()
    print("End of example")
}
```

### What are Asynchronous Sequences?
[Asynchronous Sequences](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Asynchronous-Sequences) enable you to stop an async task until the next item is prepared, giving room for other tasks to progress. Crafting your own Asynchronous Sequence involves adhering to the AsyncSequence protocol.

``` swift 
struct AsyncCounter: AsyncSequence {
    typealias Element = Int

    struct AsyncCounterIterator: AsyncIteratorProtocol {
        var count = 0
        mutating func next() async -> Int? {
            defer { count += 1 }
            return count
        }
    }

    func makeAsyncIterator() -> AsyncCounterIterator {
        return AsyncCounterIterator()
    }
}

let asyncCounter = AsyncCounter()

for await count in asyncCounter {
    print("Count: \(count)")
    try await Task.sleep(nanoseconds: 1 * 1_000_000_000)
}
```

### What are Tasks and TaskGroups?
You can draw a parallel between [Tasks](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Tasks-and-Task-Groups) and [DispatchQueue`s](https://developer.apple.com/documentation/dispatch/dispatchqueue) because they have similar concepts. If you want to execute your code asynchronously you should put your code into async context. Tasks and Queues help you solve this problem.

``` swift 
func fetchData() async throws -> String {
    try await Task.sleep(nanoseconds: 2 * 1_000_000_000)
    return "Data fetched successfully!"
}

Task {
    print("Start fetching data...")
    do {
        let result = try await fetchData()
        print(result)
    } catch {
        print("Error: \(error)")
    }
    print("Data fetching completed.")
}
```

``` swift
let customQueue = DispatchQueue(label: "com.example.myqueue", attributes: .concurrent)

func performTask() {
    customQueue.async {
        print("Task is starting...")
        Thread.sleep(forTimeInterval: 2)
        print("Task completed.")
    }
}

for _ in 1...3 {
    performTask()
}
```

[TaskGroup](https://developer.apple.com/documentation/swift/taskgroup) allows you to explicitly add child tasks and give you more control over priority and cancellation.

``` swift
func fetchImages(urls: [URL]) async throws -> [UIImage] {
    try await withThrowingTaskGroup(of: UIImage.self) { group in
        var images: [UIImage] = []
        for url in urls {
            group.addTask {
                try await downloadImage(from: url)
            }
        }
        for try await result in group {
            images.append(result)
        }
        return images
    }
}

func downloadImage(from url: URL) async throws -> UIImage {
    // Download and return the image
}
```

### What is Task.yield()?
If you have a long-running operation you can call the [`Task.yield()`](https://developer.apple.com/documentation/swift/task/yield()) method to explicitly add suspension points. By doing that you are letting other tasks make progress.

``` swift
func performAsyncOperationsWithYield() async throws {
    for index in 1...1000 {
        Task.detached {
            print("Start of operation \(index)")
            // Yield control to the scheduler
            await Task.yield()
            let result = try await someAsyncOperation(index: index)
            print("End of operation \(index) with result: \(result)")
            return result
        }
    }
}
```

### What are Sendable Types?
A type that can be shared from one concurrency context to another is known as a [sendable type](https://developer.apple.com/documentation/swift/sendable). In other words it guarantees that the operation that you perform is thread-safe.

``` swift
import Foundation

struct WeatherData: Sendable {
    var temperature: Double
    var condition: String
    var city: String
}

func fetchWeatherData(forCity city: String) async -> WeatherData {
    try? await Task.sleep(nanoseconds: 1_000_000_000)
    return WeatherData(temperature: 72.0, condition: "Sunny", city: city)
}

import SwiftUI

@MainActor
class WeatherViewModel: ObservableObject {
    @Published var currentWeather: WeatherData?

    func updateWeather(forCity city: String) {
        Task {
            let weatherData = await fetchWeatherData(forCity: city)
            // Since WeatherData conforms to Sendable, this is safe
            self.currentWeather = weatherData
        }
    }
}
```

Thank you for reading!
