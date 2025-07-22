+++
title = "LeetCode - 150 - LRU Cache"
date = 2025-07-22T07:51:33+03:00
tags = ["LeetCode", "150", "LRU Cache", "Swift"]
draft = false
+++

### The problem

Design a data structure that follows the constraints of a [Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU).

Implement the `LRUCache` class:

* `LRUCache(int capacity)` Initialize the LRU cache with positive size `capacity`.
* `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
* `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, evict the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

#### Examples

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

#### Constraints

* 1 <= capacity <= 3000
* 0 <= key <= 10^4
* 0 <= value <= 10^5
* At most 2 \* 10^5 calls will be made to get and put.

#### Explanation

Let's look at our example:
![alt image](images/146.png#center)

* From the first operation we get our capacity that equals `2`.
* Next, we have `put` operation with pairs `[1, 1]`. We will be keeping added values in order because we want to remember in what order we added these values.
* In the next operation we `put` pairs `[2, 2]`.
* At the next operation we are trying to `get` LRU value with key `1`.

The way we are going to do this is by using a hashmap, because we can look up the `key` in `O(1)` time. We also are not going to create any additional memory, because the size of our hashmap will not be exceeding our `capacity`.

We will be using our hashmap by adding the `key` that was given in our input; as for `value`, we will be using a pointer to the node.
![alt image](images/146-1.png#center)

When we execute our third `get` operation, we will be returning value `1`. When we executed the `get` operation, we made our pairs `[1, 1]` the most frequently used and pairs `[2, 2]` the least frequently used.

We are going to keep track of the most recent element and least frequently used element by using two pointers `left` and `right`, where the `left` pointer will be storing the `LRU` element and the `right` pointer will be storing the most recent element. Therefore, we are going to `swap` these two nodes.
![alt image](images/146-2.png#center)

Now we need to reorder our two nodes. The nodes will be connected with each other because we are using a doubly linked list that will allow us to swap nodes in `O(1)` time.
![alt image](images/146-3.png#center)

Next, we are going to execute our fourth `put` operation and put pairs `[3, 3]` to our hashmap. At this point we have a capacity of `2`, and when we try to add a new key, we exceed it, so we need to remove the `LRU` value and pointer to it and replace it with the new key `3` and create a new pointer.
![alt image](images/146-4.png#center)

We will continue executing steps and follow the algorithm above until we reach the end of our input.

## Doubly Linked List Solution

```swift
final class Node {
    let key: Int
    let val: Int
    var prev: Node?
    var next: Node?

    init(_ key: Int, _ val: Int) {
        self.key = key
        self.val = val
    }

}

class LRUCache {

    private let capacity: Int
    private var cache: [Int: Node]
    private var left: Node
    private var right: Node

    init(_ capacity: Int) {
        self.capacity = capacity
        self.cache = [:]
        self.left = Node(0, 0)
        self.right = Node(0, 0)
        self.left.next = self.right
        self.right.prev = self.left

    }

    func remove(_ node: Node) {
        let prev = node.prev
        let nxt = node.next
        prev?.next = nxt
        nxt?.prev = prev
    }

    func insert(_ node: Node) {
        let prev = self.right.prev
        let nxt = self.right
        prev?.next = node
        nxt.prev = node
        node.next = nxt
        node.prev = prev
    }

    func get(_ key: Int) -> Int {
        if let node = self.cache[key] {
            self.remove(node)
            self.insert(node)
            return node.val
        }
        return -1
    }

    func put(_ key: Int, _ value: Int) {
        if self.cache[key] != nil {
            self.remove(self.cache[key]!)
        }
        self.cache[key] = Node(key, value)
        self.insert(self.cache[key]!)

        if self.cache.count > self.capacity {
            let lru = self.left.next!
            self.remove(lru)
            self.cache.removeValue(forKey: lru.key)
        }
    }
}
```

#### Explanation

To solve this problem we will need an additional class `Node` that will be storing `key`, `value` pairs, and we are also going to have pointers `prev` and `next` that will point to the previous and next `Node` accordingly.

In our hashmap, which we will call `cache`, we will be mapping our `key` to `Node`.

We will also need `left` and `right` pointers that will store our `LRU` and `most recent` values.

When we call our `get` operation, we will be returning our value if it exists in our `cache`. We also should not forget to update our `LRU` and `most recent` pointers after we `get` the value. To do that, we will need two helper functions — `remove`, and `insert` — where `remove` will be removing the node from the list and `insert` will be inserting the node to the right.

When we call the `put` operation and the `key` already exists in our `cache`, we will remove that node and replace it with a new one.

Lastly, if our `cache` count exceeds the given `capacity`, we will need to remove the `LRU` value from the list and our `cache`.

#### Time/ Space complexity

* Time complexity: O(1) for each `put` and `get` operation
* Space complexity: O(n)

#### Thank you for reading! 😊
