+++
title = 'LeetCode - Blind 75 - Top K Frequent Elements'
date = 2024-11-03T08:18:58+03:00
tags = ["LeetCode", "Blind 75", "Top K Frequent Elements", "Swift"]
draft = false
+++

### The problem 
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

#### Examples
``` 
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

```
Input: nums = [1], k = 1
Output: [1]
```

#### Constraints
* 1 <= nums.length <= 10^5
* -10^4 <= nums[i] <= 10^4
* `k` is in the range [1, the number of unique elements in the array].
* It is guaranteed that the answer is unique.

Follow-up: Your algorithm's time complexity must be better than O(n log n), where `n` is the array's size.

### Brute Force Solution - Sorting
``` swift 
func topKFrequent(nums: [Int], k: Int) -> [Int] {
    var count: [Int: Int] = [:]

    for num in nums {
        if count[num] != nil {
            count[num]! += 1
        } else {
            count[num] = 1
        }
    }

    var arr: [Items] = []
    for element in count {
        let item = Items(num: element.key, cnt: element.value)
        arr.append(item)
    }

    arr.sort()

    var res: [Int] = []
    while res.count < k {
        res.append(arr.popLast()!.num)
    }

    return res
}

struct Items: Comparable {
    static func < (lhs: Items, rhs: Items) -> Bool {
        return lhs.cnt < rhs.cnt
    }
    
    let num: Int
    let cnt: Int
}
```

#### Explanation
The brute force solution:
- Iterates through all `nums` elements and determines the frequency of each element
- Based on `count`, creates `arr` that will later be sorted
- Loops through sorted `arr` and `pop`s the most frequent elements until `res.count` is less than `k`

##### Time/Space Complexity
* Time complexity: O(n log n), as it uses a sorting algorithm
* Space complexity: O(n)

### Solution 2 - Max Heap
``` swift 
import Collections

func topKFrequent(nums: [Int], k: Int) -> [Int] {
    var count: [Int: Int] = [:]

    for num in nums {
        if count[num] != nil {
            count[num]! += 1
        } else {
            count[num] = 1
        }
    }

    var maxHeap: Heap<Item> = []
    for element in count {
        maxHeap.insert(Item(num: element.key, cnt: element.value))
        if maxHeap.count > k {
            maxHeap.popMin()
        }
    }

    var res: [Int] = []
    for _ in 0 ..< k {
        res.append(maxHeap.popMax()!.num)
    }

    return res
}

struct Item: Comparable {
    static func < (lhs: Item, rhs: Item) -> Bool {
        return lhs.cnt < rhs.cnt
    }

    let num: Int
    let cnt: Int
}
``` 

#### Explanation
Solution 2:
- Iterates through input `nums` and counts the frequency of each number
- Iterates through the `count` dictionary and `insert`s elements into the heap; if the `heap` size is more than `k`, it `pop`s the minimum element 
- Iterates through a range from 0 to `k`, `pop`s the maximum elements, and `append`s them to the `res` array
- Returns the result 

##### Time/Space Complexity
* Time complexity: O(n log k)
* Space complexity: O(n + k), where `n` is the length of the input `nums` and `k` is the number of frequent elements.

### Solution 3 - Bucket Sort
``` swift 
func topKFrequent(nums: [Int], k: Int) -> [Int] {
    var count: [Int: Int] = [:]

    for num in nums {
        if count[num] != nil {
            count[num]! += 1
        } else {
            count[num] = 1
        }
    }

    var freq: [[Int]] = Array(repeating: [], count: nums.count + 1)
    for element in count {
        freq[element.value].append(element.key)
    }

    var res: [Int] = []

    for i in stride(from: freq.count - 1, to: 0, by: -1) {
        for n in freq[i] {
            res.append(n)
            if res.count == k {
                return res
            }
        }
    }

    return res
}
``` 

#### Explanation
Solution 3:
- Iterates through input `nums` and counts the frequency of each number
- Iterates through the `count` dictionary and `append`s each number by its frequency
- Iterates in reverse order, finds the numbers, and `append`s them to the `res` array
- Returns the result

##### Time/Space Complexity
* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
