+++
title = "LeetCode - Design HashSet"
date = 2026-07-08T09:41:21+03:00
tags = ["LeetCode", "Design HashSet", "Arrays_&_Hashing", "Swift"]
draft = false
+++

### The problem

Design a HashSet without using any built-in hash table libraries.

Implement `MyHashSet` class:

* `void add(key)` Inserts the value `key` into the HashSet.

* `bool contains(key)` Returns whether the value `key` exists in the HashSet.

* `void remove(key)` Removes the value `key` from the HashSet. If `key` does not exist in the HashSet, do nothing.

**Example 1:**

```
Input
["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
Output
[null, null, null, true, false, null, true, null, false]

Explanation
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // return True
myHashSet.contains(3); // return False, (not found)
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // return True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // return False, (already removed)
```

**Constraints:**

* `0 <= key <= 10^6`
* At most `10^4` calls will be made to `add`, `remove`, and `contains`.

### Solution

#### Explanation

Before jumping to the solution, let's discuss the difference between a hash set and an array.

An array can have duplicates, preserves the order in which elements were added, and allows you to access elements by index. A hash set has no order, does not allow duplicates, and does not allow you to access elements by index.

The underlying data structure of the HashSet is an **array** that uses a **hash function** to calculate the **index** of an element.

> In simple words, a hash function maps data (`key`) to an `index` in the array. In our case, the hash function uses `key` and takes it **mod** `10000` because we can have at most `10000` buckets. If we have a `key` that is larger than `10000`, **modding** maps the `key` to a value in the range from `0` to `9999`.
> For example:
> ![alt image](images/705.png#center)

The last part that we need to figure out is how to handle collisions (duplicates). There are multiple ways to do that. One of them is called `open addressing`, but in our case, we are going to focus on `chaining`.

The chaining technique uses a linked list to handle collisions. To do so, we check if the value is already in the linked list, and if it is, we skip it. For example:
![alt image](images/705-1.png#center)

#### Code

```swift
final class ListNode {
    let key: Int
    var next: ListNode?

    init(_ key: Int, _ next: ListNode? = nil) {
        self.key = key
        self.next = next
    }
}

class MyHashSet {

    private var set: [ListNode]

    init() {
        self.set = (0..<10_000).map { _ in ListNode(0) }
    }

    func add(_ key: Int) {
        let index = hash(key)
        var cur = set[index]

        while cur.next != nil {
            if cur.next!.key == key {
                return
            }
            cur = cur.next!
        }

        cur.next = ListNode(key)
    }

    func remove(_ key: Int) {
        let index = hash(key)
        var cur = set[index]

        while let next = cur.next {
            if next.key == key {
                cur.next = next.next
                return
            }
            cur = next
        }
    }

    func contains(_ key: Int) -> Bool {
        let index = hash(key)
        var cur = set[index]

        while cur.next != nil {
            if cur.next!.key == key {
                return true
            }
            cur = cur.next!
        }

        return false
    }

    func hash(_ key: Int) -> Int {
        key % set.count
    }
}
```

#### Time/ Space complexity

* Time complexity: O(n/k) for each function call
* Space complexity: O(k + m)
* Where `n` is the number of keys, `k` is the size of the set (`10000`), and `m` is the number of unique keys.

#### Thank you for reading! 😊
