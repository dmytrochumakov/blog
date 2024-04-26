+++
title = 'Combine — Basics'
date = 2024-02-07T08:29:30+03:00
tags = ["Combine"]
draft = false
+++

###  What is Combine?
Combine Framework provides an API for processing async events over time such as user-input, network response, and other dynamic data.

### What is the purpose of Combine?
The purpose of Combine is to simplify the management of async events and data streams.

### Publishers
[`Publisher`](https://developer.apple.com/documentation/combine/publisher) declares that a type can transit a sequence of values over time. A publisher delivers elements to one or more [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) instances.

``` swift
class PostService {
    func fetchPosts() -> AnyPublisher<[Post], Error> {
        guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts") else {
            fatalError("Invalid URL")
        }
        return URLSession.shared.dataTaskPublisher(for: url)
            .map(\.data)
            .decode(type: [Post].self, decoder: JSONDecoder())
            .receive(on: DispatchQueue.main)
            .eraseToAnyPublisher()
    }
}
```

### Subscribers
[`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) is a protocol that declares a type that can receive input from a publisher.
A Subscriber instance receives a stream of elements from a Publisher.

``` swift 
private var cancellable: AnyCancellable?
let service = PostService()
cancellable = service.fetchPosts()
    .sink(receiveCompletion: { completion in
        switch completion {
        case .finished:
            break
        case .failure(let error):
            print(error.localizedDescription)
        }
    }, receiveValue: { posts in
        print("Received posts count:", posts.count)
    })
```

### AnyCancellable
When you call a method like [`sink`](https://developer.apple.com/documentation/combine/publisher/sink(receivecompletion:receivevalue:)) or [`assign`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) on a publisher, it returns a type that conforms to the [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable/) protocol. Storing this return value in an instance of [`AnyCancellable`](https://developer.apple.com/documentation/combine/anycancellable/) keeps the subscription active. When the AnyCancellable instance is deallocated, its deinit method automatically cancels the subscription.

``` swift 
var cancellables = Set<AnyCancellable>()
let publisher = Just("Hello, Combine!")
publisher
    .sink { completion in
        print("Completion: \(completion)")
    } receiveValue: { value in
        print("Received value: \(value)")
    }
    .store(in: &cancellables)
```

### Operators
### Transforming Operators

- [`map`](https://developer.apple.com/documentation/combine/publishers/catch/map(_:)-6vw93): Transforms each value received from a publisher by applying a function.
- [`flatMap`](https://developer.apple.com/documentation/combine/publishers/catch/flatmap(maxpublishers:_:)-9nww4): Transforms each value received into a new publisher, then flattens the result into a single publisher stream.
- [`scan`](https://developer.apple.com/documentation/combine/publishers/catch/scan(_:_:)): Applies a closure over the previous result and the current value to produce a new value, useful for accumulating values.

### Filtering Operators

- [`filter`](https://developer.apple.com/documentation/combine/publishers/catch/filter(_:)): Emits only those values from a publisher that satisfy a given predicate.
- [`removeDuplicates`](https://developer.apple.com/documentation/combine/publishers/catch/removeduplicates()): Suppresses duplicate consecutive values published by a publisher.
- [`first/last`](https://developer.apple.com/documentation/combine/publishers/catch/last()): Emits only the first or last value from a publisher that satisfies a predicate condition.

### Combining Operators

- [`combineLatest`](https://developer.apple.com/documentation/combine/publishers/catch/combinelatest(_:_:)-2s61): Combines the latest value from two or more publishers and emits a combined value each time any of the publishers emit a value.
- [`merge`](https://developer.apple.com/documentation/combine/publishers/catch/merge(with:)): Combines multiple publishers of the same type into a single publisher stream, emitting values as they arrive.
- [`zip`](https://developer.apple.com/documentation/combine/publishers/catch/zip(_:)): Combines values from multiple publishers into tuples, emitting a tuple only when each of the publishers has emitted a new value.

### Error Handling Operators

- [`catch`](https://developer.apple.com/documentation/combine/publishers/catch/catch(_:)): Handles errors from a publisher by replacing the failed publisher with another publisher or a value.
- [`retry`](https://developer.apple.com/documentation/combine/publishers/catch/retry(_:)): Attempts to recreate a subscription to a failed publisher for a specified number of times.

### Utility Operators

- [`delay`](https://developer.apple.com/documentation/combine/publishers/catch/delay(for:tolerance:scheduler:options:)): Delays the emission of items from the publisher for a specified interval.
- [`subscribe(on:)/receive(on:)`](https://developer.apple.com/documentation/combine/publishers/catch/receive(on:options:)): Specifies the dispatch queue for performing subscription work or receiving values.
- [`print`](https://developer.apple.com/documentation/combine/publishers/catch/print(_:to:)): Prints log messages for all publisher events to the console, useful for debugging.

### Timing Operators

- [`debounce`](https://developer.apple.com/documentation/combine/publishers/catch/debounce(for:scheduler:options:)): Emits a value from a publisher only after a specified time interval has passed without receiving another value.
- [`throttle`](https://developer.apple.com/documentation/combine/publishers/catch/throttle(for:scheduler:latest:)): Emits either the first or last value received in a specified time window.

### Collecting Operators

- [`collect`](https://developer.apple.com/documentation/combine/publishers/catch/collect()): Collects received values and emits an array of those values either when the publisher completes or when a buffer size is reached.

### When to use Combine?
I found great [advice](https://developer.apple.com/documentation/combine/publisher) from Apple when it comes to Combine:

> “A Combine publisher fills a role similar to, but distinct from, the `AsyncSequence` in the Swift standard library. A `Publisher` and an `AsyncSequence` both produce elements over time. However, the pull model in Combine uses a `Subscriber` to request elements from a publisher, while Swift concurrency uses the for-await-in syntax to iterate over elements published by an `AsyncSequence`. Both APIs offer methods to modify the sequence by mapping or filtering elements, while only Combine provides time-based operations like `debounce(for:scheduler:options:)` and `throttle(for:scheduler:latest:)`, and combining operations like `merge(with:)` and `combineLatest(_:_:)`. To bridge the two approaches, the property values exposes a publisher’s elements as an `AsyncSequence`, allowing you to iterate over them with `for-await-in` rather than attaching a `Subscriber`.”

Thank you for reading!
