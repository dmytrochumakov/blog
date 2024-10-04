+++
title = 'DSA - Hashmap'
date = 2024-10-04T07:14:03+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Hashmap", "Swift"]
draft = false
+++

### What is Hashmap (aka Hash Table)?
A [hashmap](https://en.wikipedia.org/wiki/Hash_table) is a data structure that implements an associative array, also called a dictionary. An associative array maps keys to values. A hash table uses a [hash function](https://en.wikipedia.org/wiki/Hash_function) to compute an index, also called a `hash code`, into an array of `buckets or slots`, from which the desired value can be found. During a lookup, the `key` is hashed, and the resulting hash indicates where the corresponding value is stored.

Ideally, the hash function will assign each key to a unique bucket, but most hash tables are designed to employ an [imperfect hash function](https://en.wikipedia.org/wiki/Perfect_hash_function), which might cause [hash collisions](https://en.wikipedia.org/wiki/Hash_collision), where the hash function generates the same index for more than one key. Therefore, collisions must typically be accommodated in some way.

### Code Example
Here is an example in Swift, using the built-in hashmap implementation called a dictionary.
```swift
let p: [String: Int] = [
    "Michael Jordan": 23,
    "Kobe Bryant": 24,
    "LeBron James": 6
]
```

### Time/Space Complexity
#### Time complexity
| **Operation** | **Average** | **Worst case** |
|---------------|-------------|----------------|
| Search        | Θ(1)        | O(n)           |
| Insert        | Θ(1)        | O(n)           |
| Delete        | Θ(1)        | O(n)           |

#### Space complexity
| **Space** | **Average** | **Worst case** |
|-----------|-------------|----------------|
| Space     | Θ(n)        | O(n)           |

#### Thank you for reading! 😊
