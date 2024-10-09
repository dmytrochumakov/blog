+++
title = 'DSA - Hashmap - Resize'
date = 2024-10-09T07:20:29+03:00
tags = ["DSA", "Data Structures", "Algorithms", "HashMap", "Resize", "Swift"]
draft = false
+++

### Introduction
Previously, we discussed a [custom hashmap implementation](https://open.substack.com/pub/dmytrosblog/p/dsa-hashmap-custom-implementation?r=2fepxg&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true). That implementation has a lot of [collisions](https://en.wikipedia.org/wiki/Hash_collision) because we are using a fixed-size array.

If we want to reduce the chances of `collisions`, we can increase the number of slots in the hashmap, or in other words, `resize` our hashmap. Resizing cannot guarantee that collisions will be entirely eliminated, but it will help reduce the likelihood of one happening.

### Code Example
``` swift
private func loadFactor() -> Double? {
    if self.hashMap.isEmpty {
        return nil
    } else {
        var filledSlots = 0
        for slot in self.hashMap {
            if slot != nil {
                filledSlots += 1
            }
        }
        return Double(filledSlots / self.hashMap.count)
    }
}
```

``` swift
private func resize() {
    if self.hashMap.isEmpty {
        self.hashMap = [nil]
        return
    }

    guard
        let load = self.loadFactor()
    else { return }
        
    if load < 0.05 {
        return
    }

    let oldHashMap = self.hashMap
    self.hashMap = Array(repeating: nil, count: oldHashMap.count * 10)

    for kvp in oldHashMap {
        if let kvp = kvp {
            self.insert(key: kvp.key, value: kvp.value)
        }
    }
}
``` 

#### Implementation
When resizing, we create a `new hashmap` with a `larger number of slots`. Then, we `re-insert` all the key-value pairs from the `old hashmap` into the `new` one.

The resize method requires a helper function, `load factor`, which determines the number of filled buckets as a percentage of the total number of buckets.

As for the `resize` algorithm, it checks if the underlying `hashmap` is empty. If it is, it creates a `new hashmap` with a length of `1`. It then gets the `current load`, and if it's `more than 5%`, it creates a `new` empty array hashmap with `10x` the size of the current one. After that, it uses the insert method to `re-insert` all key-value pairs from the `old` hashmap into the `new` one.

The final step is to update the `insert` method and perform a `resize` check before inserting to ensure there is enough space.

### Complete Code Example
``` swift 
final class HashMap<Key: StringProtocol, Value> {

    private(set) var hashMap: [(key: Key, value: Value)?]

    init(size: Int) {
        self.hashMap = Array(repeating: nil, count: size)
    }

    func getValue(by key: Key) -> Value? {
        let index = hashFunction(key: key)
        return self.hashMap[index]?.value
    }

    func insert(key: Key, value: Value) {
        self.resize()
        let index = hashFunction(key: key)
        self.hashMap[index] = (key, value)
    }

    private func hashFunction(key: Key) -> Int {
        var count = 0
        for element in key.unicodeScalars {
            count += Int(element.value)
        }
        return count % self.hashMap.count
    }

    private func resize() {
        if self.hashMap.isEmpty {
            self.hashMap = [nil]
            return
        }

        guard
            let load = self.loadFactor()
        else { return }
        
        if load < 0.05 {
            return
        }

        let oldHashMap = self.hashMap
        self.hashMap = Array(repeating: nil, count: oldHashMap.count * 10)

        for kvp in oldHashMap {
            if let kvp = kvp {
                self.insert(key: kvp.key, value: kvp.value)
            }
        }
    }

    private func loadFactor() -> Double? {
        if self.hashMap.isEmpty {
            return nil
        } else {
            var filledSlots = 0
            for slot in self.hashMap {
                if slot != nil {
                    filledSlots += 1
                }
            }
            return Double(filledSlots / self.hashMap.count)
        }
    }

}
```

#### Thank you for reading! 😊
