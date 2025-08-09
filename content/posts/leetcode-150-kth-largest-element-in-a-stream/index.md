+++
title = "LeetCode - 150 - Kth Largest Element in a Stream"
date = 2025-08-09T07:41:18+03:00
tags = ["LeetCode", "150", "Kth Largest Element in a Stream", "Swift"]
draft = false
+++

### The problem

You are part of a university admissions office and need to keep track of the `kth` highest test score from applicants in real-time. This helps to determine cut-off marks for interviews and admissions dynamically as new applicants submit their scores.

You are tasked to implement a class which, for a given integer `k`, maintains a stream of test scores and continuously returns the `kth` highest test score after a new score has been submitted. More specifically, we are looking for the `kth` highest score in the sorted list of all scores.

Implement the `KthLargest` class:

* `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of test scores `nums`.
* `int add(int val)` Adds a new test score `val` to the stream and returns the element representing the `kth` largest element in the pool of test scores so far.

#### Examples

```
Input:
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]

Output: [null, 4, 5, 5, 8, 8]

Explanation:

KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3); // return 4
kthLargest.add(5); // return 5
kthLargest.add(10); // return 5
kthLargest.add(9); // return 8
kthLargest.add(4); // return 8
```

```
Input:
["KthLargest", "add", "add", "add", "add"]
[[4, [7, 7, 7, 7, 8, 3]], [2], [10], [9], [9]]

Output: [null, 7, 7, 7, 8]

Explanation:

KthLargest kthLargest = new KthLargest(4, [7, 7, 7, 7, 8, 3]);
kthLargest.add(2); // return 7
kthLargest.add(10); // return 7
kthLargest.add(9); // return 7
kthLargest.add(9); // return 8
```

#### Constraints

* 0 <= nums.length <= 10^4
* 1 <= k <= nums.length + 1
* -10^4 <= nums\[i] <= 10^4
* -10^4 <= val <= 10^4
* At most 10^4 calls will be made to add.

#### Explanation

From the description of the problem, we learn that we need to design the class to find the `kth` largest element in a stream of numbers. 

In this case, a stream of numbers means that we continue adding numbers to the list of numbers and after that, we need to find the `kth` element. 

The problem description also mentions that we need to find the `kth` largest element in sorted order, not distinct elements. 

This means that if, for example, we had input with `k = 3, nums = [1, 2, 2, 4]` and we needed to return a `distinct` element, we would return the value `1`, but in our case, we need to return the `kth` largest element in sorted order, therefore we would return `2`, because we can include multiple copies of the same element.

### Sorting Solution

```swift
class KthLargest {

    private let k: Int
    private var nums: [Int]

    init(_ k: Int, _ nums: [Int]) {
        self.k = k
        self.nums = nums
    }

    func add(_ val: Int) -> Int {
        self.nums.append(val)
        self.nums.sort()
        return self.nums[self.nums.count - self.k]
    }
}
```

#### Explanation

The brute force way to solve this problem is by using sorting.

Every time we `add` a new element, we will sort the array and find our `kth` element, but it’s not a very efficient approach because the time complexity of the `add` operation will be `O(m*n*log(n))`. We can optimize it by using a different data structure.

#### Time/ Space complexity

* Time complexity: O(m*n*log(n))
* Space complexity: O(m)
* Where `m` is the number of calls made to `add`, and `n` is the current size of the array

### Min Heap Solution

```swift
class KthLargest {

    private let k: Int
    private var minHeap: Heap<Int>

    init(_ k: Int, _ nums: [Int]) {
        self.k = k
        self.minHeap = Heap(nums)
        while minHeap.count > k {
            minHeap.popMin()
        }
    }

    func add(_ val: Int) -> Int {
        minHeap.insert(val)
        if minHeap.count > k {
            minHeap.popMin()
        }
        return minHeap.min!
    }

}
```

#### Explanation

The more optimal way to solve this problem is to use the `Min Heap` data structure.

* The min heap approach is better than sorting because `add`ing and `pop`ping operations take `O(logn)` time, and finding the `min` element takes `O(1)` time, and we know that we will only be adding new elements.
* As for the size of our min heap, it will be equal to the size of input `k`, because if we look at the example with input `k=3, nums=[5, 6, 9, 3]`
  ![alt image](images/703.png#center)
  we can see that we only need `k` largest values from the array. In our case, the values are `5, 6, 9`. We are never going to include the value `3` because it cannot be the largest value as it’s less than every value in `5, 6, 9`.

In case we had the value `7` in our input, our sequence would look like this `6, 7, 9`, because the value `5` is less than any of those values. Therefore, we now have a new `k` largest sequence of elements.
![alt image](images/703-1.png#center)

In case in our input we had the value `2`, it would not affect our result because it is less than any element in our min heap.

#### Time/ Space complexity

* Time complexity: O(m\*logk)
* Space complexity: O(k)
* Where `m` is the number of calls made to `add`

#### Thank you for reading! 😊
