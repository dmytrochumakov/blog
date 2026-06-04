+++
title = "LeetCode - 150 - Minimum Interval to Include Each Query"
date = 2026-06-04T09:37:51+03:00
tags = ["LeetCode", "150", "Minimum Interval to Include Each Query", "Intervals", "Swift"]
draft = false
+++

### The problem

You are given a 2D integer array `intervals`, where `intervals[i] = [lefti, righti]` describes the `ith` interval starting at `lefti` and ending at `righti` **(inclusive)**. The **size** of an interval is defined as the number of integers it contains, or more formally `righti - lefti + 1`.

You are also given an integer array `queries`. The answer to the `jth` query is the **size of the smallest interval** `i` such that `lefti <= queries[j] <= righti`. If no such interval exists, the answer is `-1`.

Return *an array containing the answers to the queries*.

**Example 1:**

```
Input: intervals = [[1,4],[2,4],[3,6],[4,4]], queries = [2,3,4,5]
Output: [3,3,1,4]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,4] is the smallest interval containing 2. The answer is 4 - 2 + 1 = 3.
- Query = 3: The interval [2,4] is the smallest interval containing 3. The answer is 4 - 2 + 1 = 3.
- Query = 4: The interval [4,4] is the smallest interval containing 4. The answer is 4 - 4 + 1 = 1.
- Query = 5: The interval [3,6] is the smallest interval containing 5. The answer is 6 - 3 + 1 = 4.
```

**Example 2:**

```
Input: intervals = [[2,3],[2,5],[1,8],[20,25]], queries = [2,19,5,22]
Output: [2,-1,4,6]
Explanation: The queries are processed as follows:
- Query = 2: The interval [2,3] is the smallest interval containing 2. The answer is 3 - 2 + 1 = 2.
- Query = 19: None of the intervals contain 19. The answer is -1.
- Query = 5: The interval [2,5] is the smallest interval containing 5. The answer is 5 - 2 + 1 = 4.
- Query = 22: The interval [20,25] is the smallest interval containing 22. The answer is 25 - 20 + 1 = 6.
```

**Constraints:**

* `1 <= intervals.length <= 105`
* `1 <= queries.length <= 105`
* `intervals[i].length == 2`
* `1 <= lefti <= righti <= 107`
* `1 <= queries[j] <= 107`

#### Explanation

The brute force way to solve this problem is to create two nested loops and iterate over `queries` and `intervals`. This way, we can find the smallest interval and calculate the answer to the query. This is not a very efficient solution because it will take `O(n * q)` time, but we can slightly optimize it.

### Solution

#### Explanation

To make the solution more efficient, we are going to sort `intervals` and `queries`. This way, we can process intervals in order and determine which ones match the query.

![alt image](images/1851.png#center)

> To avoid losing the order in which `queries` were initially given, we are going to use a hashmap to store the index and value of the query it belongs to.

When we find a matching interval, we are going to calculate its **size** and add it to a Min Heap.

When we have intervals of the same size, we are going to use the **end** property as a tie-breaker in the Min Heap. This way, we can ensure that we can always find the minimum interval even if there is a tie between them.

For example, if we had intervals `[0, 1]` and `[1, 2]`, the size of both intervals would be `2`, and we are always going to look for the interval with the smallest **end** value because that interval appears earlier.

![alt image](images/1851-1.png#center)

While iterating, we are going to find all intervals that the query belongs to:

* calculate their **size**
* add them to the Min Heap
* and use them to find the minimum result.

![alt image](images/1851-2.png#center)

The last case that we need to take care of is when we **pop** elements from the Min Heap.

* We are going to create a while loop and **pop** values from the Min Heap where the **end** value is less than the current query.

For example, with `query = 5`, we are going to **pop** `(1, 4)`, `(3, 4)`, `(4, 4)` and stay only with `(4, 6)`, which we are going to add to the result.

![alt image](images/1851-3.png#center)

#### Code

```swift
struct Helper: Comparable {

    let size: Int
    let r: Int

    static func < (lhs: Helper, rhs: Helper) -> Bool {
        if lhs.size == rhs.size {
            return lhs.r < rhs.r
        }
        return lhs.size < rhs.size
    }

}

func minInterval(_ intervals: [[Int]], _ queries: [Int]) -> [Int] {
    let intervals = intervals.sorted { $0[0] < $1[0] }
    var minHeap: Heap<Helper> = []
    var i = 0
    var res: [Int: Int] = [:]

    for q in queries.sorted() {
        while i < intervals.count && intervals[i][0] <= q {
            let l = intervals[i][0]
            let r = intervals[i][1]
            minHeap.insert(Helper(size: r - l + 1, r: r))
            i += 1
        }

        while !minHeap.isEmpty && minHeap.min!.r < q {
            minHeap.removeMin()
        }

        if !minHeap.isEmpty {
            res[q] = minHeap.min!.size
        } else {
            res[q] = -1
        }
    }

    var output: [Int] = []
    for q in queries {
        output.append(res[q]!)
    }

    return output
}
```

#### Time/ Space complexity

* Time complexity: `O(n * log n + m * log n)`
* Space complexity: `O(n + m)`
* Where `m` is the length of `queries` and `n` is the length of `intervals`.

#### Thank you for reading! 😊
