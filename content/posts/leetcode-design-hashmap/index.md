+++
title = "LeetCode - Design HashMap"
date = 2026-07-18T08:52:24+03:00
tags = ["LeetCode", "Design HashMap", "Arrays_&_Hashing", "Swift"]
draft = false
+++

### The problem

Design a HashMap without using any built-in hash table libraries.

Implement the `MyHashMap` class:

* `MyHashMap()` initializes the object with an empty map.

* `void put(int key, int value)` inserts a `(key, value)` pair into the HashMap. If the `key` already exists in the map, update the corresponding `value`.

* `int get(int key)` returns the `value` to which the specified `key` is mapped, or `-1` if this map contains no mapping for the `key`.

* `void remove(key)` removes the `key` and its corresponding `value` if the map contains the mapping for the `key`.

**Example 1:**

```
Input
["MyHashMap", "put", "put", "get", "get", "put", "get", "remove", "get"]
[[], [1, 1], [2, 2], [1], [3], [2, 1], [2], [2], [2]]
Output
[null, null, null, 1, -1, null, 1, null, -1]

Explanation
MyHashMap myHashMap = new MyHashMap();
myHashMap.put(1, 1); // The map is now [[1,1]]
myHashMap.put(2, 2); // The map is now [[1,1], [2,2]]
myHashMap.get(1);    // return 1, The map is now [[1,1], [2,2]]
myHashMap.get(3);    // return -1 (i.e., not found), The map is now [[1,1], [2,2]]
myHashMap.put(2, 1); // The map is now [[1,1], [2,1]] (i.e., update the existing value)
myHashMap.get(2);    // return 1, The map is now [[1,1], [2,1]]
myHashMap.remove(2); // remove the mapping for 2, The map is now [[1,1]]
myHashMap.get(2);    // return -1 (i.e., not found), The map is now [[1,1]]
```

**Constraints:**

* `0 <= key, value <= 10^6`
* At most `10^4` calls will be made to `put`, `get`, and `remove`.

### Linked List Solution

#### Explanation

Before diving into the code, let's discuss what a HashMap is.

A HashMap is a data structure that can store key-value data. The `key`s are always unique and are not allowed to have any duplicates. 

A HashMap uses an array for storing elements, and to calculate the `index`, it uses a hash function.

Simply put, a hash function takes a `key` and transforms it into an integer that can be used as an `index` in the array. For example:
![alt image](images/706.png#center)

We are going to use a simple hash function where the `key` is going to be **mod**ded by the size of the array. The size of the array is going to be `10000`.
![alt image](images/706-1.png#center)

Lastly, we need to talk about collisions.

There are multiple ways to handle collisions. One of them is called *open addressing*, and another one is called *chaining*. In this problem, we are going to focus on the **chaining** technique.

The chaining technique uses a Linked List to store multiple key-value pairs for a calculated **index**. 

In order for this solution to work, instead of just storing the `value`, we are going to store one additional property: the `key`. This way, when we have the same index, we can check whether the `key` already exists and update its `value`; otherwise, we store a new `key`-`value` pair. For example:
![alt image](images/706-3.png#center)

#### Code

```swift
final class ListNode {
    let key: Int
    var val: Int
    var next: ListNode?
    init(key: Int = -1, val: Int = -1, next: ListNode? = nil) {
        self.key = key
        self.val = val
        self.next = next
    }
}

class MyHashMap {

    private let hashmap: [ListNode]

    init() {
        self.hashmap = Array(repeating: ListNode(), count: 10000)
    }

    func put(_ key: Int, _ value: Int) {
        let index = self.hash(key)
        var cur: ListNode? = self.hashmap[index]

        while cur?.next != nil {
            if cur?.next?.key == key {
                cur?.next?.val = value
                return
            }
            cur = cur?.next
        }

        cur?.next = ListNode(key: key, val: value)
    }

    func get(_ key: Int) -> Int {
        let index = self.hash(key)
        var cur = self.hashmap[index].next

        while cur != nil {
            if cur?.key == key {
                return cur!.val
            }
            cur = cur?.next
        }

        return -1
    }

    func remove(_ key: Int) {
        let index = self.hash(key)
        var cur: ListNode? = self.hashmap[index]

        while cur != nil && cur?.next != nil {
            if cur?.next?.key == key {
                cur?.next = cur?.next?.next
                return
            }
            cur = cur?.next
        }
    }

    func hash(_ key: Int) -> Int {
        if self.hashmap.count == 0 {
            return 0
        } else {
            return key % self.hashmap.count
        }
    }

}
```

#### Time/ Space complexity

* Time complexity: O(n/k) for each function call
* Space complexity: O(k + m)
* Where `n` is the number of keys, `k` is the size of the map (`10000`), and `m` is the number of unique keys.

#### Thank you for reading! 😊
