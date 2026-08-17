+++
title = "LeetCode - Sort an Array"
date = 2026-08-17T07:00:00+03:00
tags = ["LeetCode", "Sort an Array", "Arrays_&_Hashing", "Medium", "Swift"]
draft = false
+++

### The problem

Given an array of integers `nums`, sort the array in ascending order and return it.

You must solve the problem **without using any built-in** functions in `O(nlog(n))` time complexity and with the smallest space complexity possible.

**Example 1:**

```
Input: nums = [5,2,3,1]
Output: [1,2,3,5]
Explanation: After sorting the array, the positions of some numbers are not changed (for example, 2 and 3), while the positions of other numbers are changed (for example, 1 and 5).

```

**Example 2:**

```
Input: nums = [5,1,1,2,0,0]
Output: [0,0,1,1,2,5]
Explanation: Note that the values of nums are not necessarily unique.

```

**Constraints:**

* `1 <= nums.length <= 5 * 10^4`
* `-5 * 10^4 <= nums[i] <= 5 * 10^4`

#### Explanation

There are not so many sorting algorithms that have O(n*logn) time complexity. You can look at [comparison](https://www.geeksforgeeks.org/dsa/time-complexities-of-all-sorting-algorithms/) and see that only Heap Sort, Merge Sort, Tim Sort and Cube Sort have that O(n*logn) time. I have never implemented Tim Sort or Cube Sort myself, so I will be focusing on Heap Sort. But you always can try it for yourself; here are the links [Tim Sort](https://www.geeksforgeeks.org/dsa/timsort/), [Cube Sort](https://n64squid.com/projects/sort/insertion/cube-sort/).

My first thought to solve this problem was to use Min Heap. It has *heapify* method that helps build a *Binary Tree* and *move minimum value up to the root* so that when you **pop** the value you always get the minimum one.

The last step would be to *loop* through the elements and *pop* them while *adding* to the result. This way you can always *pop* the minimum value and create an ordered list.

This is a working solution, but it has two issues:

* First, it allocates additional memory that we want to reduce
* Second, it uses built-in functions that are not allowed by this problem

#### Code

```swift
func sortArray(_ nums: [Int]) -> [Int] {
    var minHeap: Heap<Int> = Heap(nums)
    var res: [Int] = []
    
    for _ in 0 ..< nums.count {
        res.append(minHeap.removeMin())
    }
    
    return res
}
```

### Heap Solution

#### Explanation

The initial idea of using a heap was close, but it can be done in the opposite way (by using Max Heap) with less memory footprint.

The first phase is to build Max Heap. We can do that by skipping leaf nodes (which have no children), starting from the last non-leaf node, working our way (up) to the root and finding the max value. All that can be done by *for loop* and *heapify* method.

#### Phase 1

```swift
func sortArray(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var nums = nums
    
    // Phase 1
    buildMaxHeap(&nums, n)
    
    return nums
}

func buildMaxHeap(_ nums: inout [Int], _ n: Int) {
    for i in stride(from: n / 2 - 1, to: -1, by: -1) {
        heapify(&nums, n , i)
    }
}
```

Since *Max Heap* is built using *[complete](https://en.wikipedia.org/wiki/Binary_tree#complete) Binary Tree*, we can easily access the left and right child nodes by their corresponding indices `(2*i + 1)`, `(2*i + 2)`. The *heapify* method in this case looks for the *largestNode* index, and if we find one, we *swap* the current value with the *largest* one.

Once the Heap is ready, we can repeatedly extract the largest element and sort the array in place.

#### Phase 2

```swift
func sortArray(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var nums = nums
    
    // Phase 1
    buildMaxHeap(&nums, n)

    // Phase 2
    sortArray(&nums, n)
    
    return nums
}

func sortArray(_ nums: inout [Int], _ n: Int) {
    for i in stride(from: n - 1, to: 0, by: -1) {
        nums.swapAt(0, i)
        heapify(&nums, i , 0)
    }
}
```

#### Completed Code

```swift
func sortArray(_ nums: [Int]) -> [Int] {
    let n = nums.count
    var nums = nums
    
    // Phase 1
    buildMaxHeap(&nums, n)
    
    // Phase 2
    sortArray(&nums, n)
    
    return nums
}

func buildMaxHeap(_ nums: inout [Int], _ n: Int) {
    for i in stride(from: n / 2 - 1, to: -1, by: -1) {
        heapify(&nums, n , i)
    }
}

func sortArray(_ nums: inout [Int], _ n: Int) {
    for i in stride(from: n - 1, to: 0, by: -1) {
        nums.swapAt(0, i)
        heapify(&nums, i , 0)
    }
}

func heapify(_ arr: inout [Int], _ n: Int, _ i: Int)  {
    let l = 2*i + 1
    let r = 2*i + 2
    var largestNode = i
    
    if l < n && arr[l] > arr[largestNode] {
        largestNode = l
    }
    
    if r < n && arr[r] > arr[largestNode] {
        largestNode = r
    }
    
    if largestNode != i {
        arr.swapAt(i, largestNode)
        heapify(&arr, n , largestNode)
    }
}
```

#### Time/ Space complexity

* Time complexity: O(n*logn)
* Space complexity: O(logn) for recursive stack

#### Thank you for reading! 😊

