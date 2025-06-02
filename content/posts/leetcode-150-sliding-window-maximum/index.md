+++
title = "LeetCode - 150 - Sliding Window Maximum"
date = 2025-06-02T07:18:22+03:00
tags = ["LeetCode", "150", "Sliding Window Maximum", "Swift"]
draft = false
+++

### The problem

You are given an array of integers `nums`, and there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

#### Examples

```
Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

```
Input: nums = [1], k = 1
Output: [1]
```

#### Constraints

* 1 <= nums.length <= 10^5
* -10^4 <= nums\[i] <= 10^4
* 1 <= k <= nums.length

#### Explanation

Before we jump into the solution, let's look at our example with `nums = [1,3,-1,-3,5,3,6,7], k = 3`
![alt image](images/239.png#center)
We can see that at the first position we have **max** value `3`, which we add to our output array.

* At the next position, we see that our **max** is still `3`, so we put it in our output.
* At the next position, we see that our **max** value is `5`, which we put in our output.

You might recognize one solution to this problem immediately, by finding **max** in the window array.

### Brute Force Solution

```swift
func maxSlidingWindow(_ nums: [Int], _ k: Int) -> [Int] {
    var res: [Int] = []
    let n = nums.count

    for i in 0 ..< n - k + 1 {
        var maxVal = nums[i]

        for j in i ..< i + k {
            maxVal = max(maxVal, nums[j])
        }

        res.append(maxVal)
    }

    return res
}
```

#### Explanation

Above we learn that we can solve this problem in a brute force way by going through each window and scanning through the array and finding the **max** value.
This solution is not very efficient because we have repetitive work when we scan the array. But we can optimize it even further and get O(n) time.

#### Time / Space complexity

* Time complexity: O(k \* n)
* Space complexity: O(1) extra space, O(n - k + 1) for the output array
* Where `k` is the size of the window, and `n` is the length of the input array.

### Deque Solution

```swift
func maxSlidingWindow(_ nums: [Int], _ k: Int) -> [Int] {
    var res: [Int] = []

    var q: Deque<Int> = Deque()
    var l = 0
    var r = 0

    while r < nums.count {
        while !q.isEmpty && nums[q.last!] < nums[r] {
            q.removeLast()
        }

        q.append(r)

        if l > q[0] {
            q.removeFirst()
        }

        if (r + 1) >= k {
            res.append(nums[q[0]])
            l += 1
        }

        r += 1
    }

    return res
}
```

#### Explanation

From the brute force solution we learn that we can eliminate repetitive work.
![alt image](images/239-1.png#center)

* If we have a window and we see a value that is greater than the previous values in our window, then we can eliminate those values from our window. We can do this by using the Deque data structure.
* Values in deque are always going to be in decreasing order.

![alt image](images/239-2.png#center)
Since value `4` is greater than the rightmost position in our deque, we can pop it.

* After that, we are going to make a comparison again. Value `4` is still at the top of our deque, so we are going to pop it.
* We will continue popping until we have popped all elements.
* Next, we will add value `4` to our output.

![alt image](images/239-3.png#center)
Now, we shift our window by one position, and we introduce a new element `5`.

* We need to check if `5` is greater than the value at the top of our deque.
* After the comparison, we see that `4 < 5`, so we remove value `4` from our deque and add value `5`.
* We also add value `5` to our output.

This solution is more efficient because adding and removing elements from a deque takes O(1) time, so the overall time complexity will be O(n).

> This type of problem is called a monotonic decreasing queue. The reason behind this is that our queue is always going to be in decreasing order.

#### Time / Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
