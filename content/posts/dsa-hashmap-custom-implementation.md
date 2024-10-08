+++
title = 'DSA - HashMap - Custom Implementation'
date = 2024-10-07T07:01:20+03:00
tags = ["DSA", "Data Structures", "Algorithms", "HashMap", "Swift"]
draft = false
+++

### Introduction

Let's take a closer look at a HashMap implementation by building it without using the built-in dictionary in Swift. The HashMap will use a fixed-size array underneath the hash function that will transform the `key` into an `index`. In this example, the hash function is based on taking the modulo of the sum of all `key.unicodeScalars` integer values with the size of the array.

### Code Example
```swift
final class HashMap<Key: StringProtocol, Value> {

    private var hashMap: [(key: Key, value: Value)?]
    private var size: Int

    init(size: Int) {
        self.hashMap = Array(repeating: nil, count: size)
        self.size = size
    }

    private func hashFunction(key: Key) -> Int {
        var count = 0
        for element in key.unicodeScalars {
            count += Int(element.value)
        }
        return count % self.size
    }

}
```

### Insert
The insert operation uses the `index` provided by the `hashFunction` and sets the value at this `index`.
```swift
func insert(key: Key, value: Value) {
    let index = hashFunction(key: key)
    self.hashMap[index] = (key, value)
}
```

### Get
The get operation uses the `index` provided by the `hashFunction` and retrieves the value at this `index`.
```swift
func getValue(by key: Key) -> Value? {
    let index = hashFunction(key: key)
    return self.hashMap[index]?.value
}
```

### Complete Code Example
```swift
final class HashMap<Key: StringProtocol, Value> {

    private var hashMap: [(key: Key, value: Value)?]
    private var size: Int

    init(size: Int) {
        self.hashMap = Array(repeating: nil, count: size)
        self.size = size
    }

    func getValue(by key: Key) -> Value? {
        let index = hashFunction(key: key)
        return self.hashMap[index]?.value
    }

    func insert(key: Key, value: Value) {
        let index = hashFunction(key: key)
        self.hashMap[index] = (key, value)
    }

    private func hashFunction(key: Key) -> Int {
        var count = 0
        for element in key.unicodeScalars {
            count += Int(element.value)
        }
        return count % self.size
    }

}
```

### Time/Space Complexity
Time complexity in Big O notation:

| Operation | Average | Worst Case |
|-----------|---------|------------|
| Insert    | Θ(1)    | O(n)       |

Space complexity:
Θ(n) 

#### Thank you for reading! 😊
