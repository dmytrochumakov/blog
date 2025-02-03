+++
title = 'LeetCode - Blind 75 - Find Median from Data Stream'
date = 2025-02-03T11:28:36+03:00
tags = ["LeetCode", "Blind 75", "Find Median from Data Stream", "Swift"]
draft = false
+++

### The Problem  
> The **median** is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.  
* For example, for `arr = [2,3,4]`, the median is `3`.  
* For example, for `arr = [2,3]`, the median is `(2 + 3) / 2 = 2.5`.  

Implement the `MedianFinder` class:  
* `MedianFinder()` initializes the `MedianFinder` object.  
* `void addNum(int num)` adds the integer `num` from the data stream to the data structure.  
* `double findMedian()` returns the median of all elements so far. Answers within `10⁻⁵` of the actual answer will be accepted.  

#### Examples  
```swift
Input
["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]

Output
[null, null, null, 1.5, null, 2.0]

Explanation
MedianFinder medianFinder = new MedianFinder();
medianFinder.addNum(1);    // arr = [1]
medianFinder.addNum(2);    // arr = [1, 2]
medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
medianFinder.addNum(3);    // arr = [1, 2, 3]
medianFinder.findMedian(); // return 2.0
```

#### Constraints  
* `-10⁵ <= num <= 10⁵`  
* There will be at least one element in the data structure before calling `findMedian()`.  
* At most `5 * 10⁴` calls will be made to `addNum` and `findMedian()`.  

#### Follow-up:  
* If all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?  
* If `99%` of all integer numbers from the stream are in the range `[0, 100]`, how would you optimize your solution?  

---

## Sorting Solution  
```swift
class MedianFinder {

    private var data: [Int]

    init() {
        self.data = []
    }

    func addNum(_ num: Int) {
        data.append(num)
    }

    func findMedian() -> Double {
        data.sort()
        let n = data.count

        if (n & 1) != 0 {
            return Double(data[n / 2])
        } else {
            return (Double(data[n / 2]) + Double(data[n / 2 - 1])) / 2
        }
    }

}
```

#### Explanation  
One approach to solving this problem is to apply a built-in sorting algorithm. To find the result, we use an array to collect all numbers added using the `addNum` method.  

Before finding the median, we should sort the array first because the main requirement for calculating the median is an **ordered list**. After sorting, we check whether the length of `data` is **odd** or **even** and calculate the median accordingly.  

This solution is not very efficient and takes **O(m * n log n)** time because we are using a built-in sorting algorithm. We can find a solution that is much faster.  

#### Time/Space Complexity  
* **Time Complexity**: O(m) for `addNum`, O(m * n log n)` for `findMedian`  
* **Space Complexity**: O(n)  
* Where `m` is the number of function calls and `n` is the length of the array.  

---

## Heap Solution  
```swift
class MedianFinder {

    private var small: Heap<Int>
    private var large: Heap<Int>

    init() {
        self.small = Heap<Int>(sort: >)  // Max-Heap
        self.large = Heap<Int>(sort: <)  // Min-Heap
    }

    func addNum(_ num: Int) {
        if !large.isEmpty && num > large.min! {
            large.insert(num)
        } else {
            small.insert(num)
        }

        let smallCount = small.count
        let largeCount = large.count

        if smallCount > largeCount + 1 {
            let val = small.popMax()!
            large.insert(val)
        }

        if largeCount > smallCount + 1 {
            let val = large.popMin()!
            small.insert(val)
        }
    }

    func findMedian() -> Double {
        let smallCount = small.count
        let largeCount = large.count

        if smallCount > largeCount {
            return Double(small.max!)
        }
        if largeCount > smallCount {
            return Double(large.min!)
        }

        return Double(small.max! + large.min!) / 2
    }

}
```

#### Explanation  
A more optimal way to solve this problem is by using **Min-Heap** and **Max-Heap**. We use additional collections from the Apple Swift package that provide a built-in **Heap** data structure. A heap can behave as either a **Min-Heap** or a **Max-Heap**.  

The **Heap solution is more efficient** because it can insert elements in **O(log n)** time and find the **min** or **max** in **O(1)** time.  

To solve this problem, we need **two heaps**:  
1. **Small Heap (Max-Heap)**  
2. **Large Heap (Min-Heap)**  

We insert elements into one of the heaps, and if the condition **small heap size ≤ large heap size** is not satisfied, we move elements accordingly.  

For example:  
* `small = [2, 7], large = [3]`  
  - We find the **max** in `small` and remove it.  
  - We **add** it to `large`, so now `small = [2]`, `large = [3, 7]`.  

Another condition:  
* If `small` heap size is **less** than `large` heap size:  
  - We find the **min** in `large`, remove it, and **add** it to `small`.  
  - Example: `small = [2], large = [3, 7, 4]` → `small = [2, 3], large = [7, 4]`.  

We calculate the **median** by finding the **max** value in `small` and **min** value in `large`.  

#### Time/Space Complexity  
* **Time Complexity**: O(m * log n) for `addNum`, O(m) for `findMedian`  
* **Space Complexity**: O(n)  
* Where `m` is the number of function calls and `n` is the length of the array.  

---

#### Thank you for reading! 😊
