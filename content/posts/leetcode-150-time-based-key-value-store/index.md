+++
title = "LeetCode - 150 - Time Based Key-Value Store"
date = 2025-07-06T07:42:17+03:00
tags = ["LeetCode", "150", "Time Based Key-Value Store", "Swift"]
draft = false
+++

### The problem

Design a time-based key-value data structure that can store multiple values for the same key at different timestamps and retrieve the key's value at a certain timestamp.

Implement the `TimeMap` class:

* `TimeMap()` Initializes the object of the data structure.
* `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
* `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

#### Examples

```
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "bar2" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
```

#### Constraints

* 1 <= key.length, value.length <= 100
* key and value consist of lowercase English letters and digits.
* 1 <= timestamp <= 10^7
* All the timestamps of set are strictly increasing.
* At most 2 \* 10^5 calls will be made to set and get.

#### Explanation

Our objective for this task is to design a key-value store.
We are going to have a `key`, and a list of `values` associated with that key, and each value is going to have a `timestamp` associated with it.
As for operations, we are going to support only `set` and `get` operations.
Now, let's look at our example
![alt image](images/981.png#center)

* we are going to `set`, `foo` as our `key`, and `[bar, 1]` as our value
* next, we are going to `get` value by `key = “foo”` and `timestamp = 1`, so we will be returning value `"bar"`

We will be solving this problem by using a hashmap. Our `key` will be our actual key that is given to us as an input, and our `value` will be a list of pairs (`value`, `timestamp`). To find our result we will be iterating over the list of pair values.
  Let's move to our second `get` operation

* we are given `key = "foo"`, and `timestamp = 3`, so we are going to go to the same list as we did before.
  You can see that we don't have a value with `timestamp = 3`, but we are not going to return `nil`, because of how this problem is defined. 
  
The problem does not want to find an exact match; instead, we need to find and return the most recent one. By recent, they mean the closest result (`timestamp_prev <= timestamp`).

Next, we will continue doing the same steps as we did above until we complete all of our operations.
The `set` operation will always take O(1) time, but for the `get` operation we can do it in multiple ways and with different time complexity.

### Brute Force Solution

```swift
class TimeMap {
    private var store: [String: [(String, Int)]]
    init() {
        self.store = [:]
    }

    func set(_ key: String, _ value: String, _ timestamp: Int) {
        if self.store[key] == nil {
            self.store[key] = []
        }
        self.store[key]!.append((value, timestamp))
    }

    func get(_ key: String, _ timestamp: Int) -> String {
        var res = ""
        let values = self.store[key] ?? []

        for value in values {
            if value.1 <= timestamp {
                res = value.0
            }
        }

        return res
    }
}
```

#### Explanation

One way to solve this problem is to just iterate over the list of all pairs and find the closest one. This solution will take O(n), but we know that we are searching for values, so we can use binary search that will take O(logn) time.

#### Time/Space complexity

* Time complexity: O(1) for set operation, O(n) for get operation
* Space complexity: O(m\*n)
* where `n` is the total number of unique timestamps associated with a key, and `m` is the total number of keys

### Binary Search Solution

```swift
class TimeMap {
    private var store: [String: [(String, Int)]]
    init() {
        self.store = [:]
    }

    func set(_ key: String, _ value: String, _ timestamp: Int) {
        if self.store[key] == nil {
            self.store[key] = []
        }
        self.store[key]!.append((value, timestamp))
    }

    func get(_ key: String, _ timestamp: Int) -> String {
        var res = ""
        let values = self.store[key] ?? []

        var l = 0
        var r = values.count - 1

        while l <= r {
            let m = (l + r) / 2
            if values[m].1 <= timestamp {
                res = values[m].0
                l = m + 1
            } else {
                r = m - 1
            }
        }

        return res
    }
}
```

#### Explanation

A more optimal way to solve this problem is to use binary search. We can use it without additional sorting because in the constraints section they mention that `All timestamps of set operation are strictly increasing`, so we do not need to sort it.

#### Time/Space complexity

* Time complexity: O(1) for set operation, and O(logn) for get operation
* Space complexity: O(m\*n)

#### Thank you for reading! 😊
