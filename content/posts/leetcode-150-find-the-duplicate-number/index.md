+++
title = "LeetCode - 150 - Find the Duplicate Number"
date = 2025-07-17T07:55:28+03:00
tags = ["LeetCode", "150", "Find the Duplicate Number", "Swift"]
draft = false
+++

### The problem

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only one repeated number in `nums`, return this repeated number.

You must solve the problem without modifying the array `nums` and using only constant extra space.

#### Examples

```
Input: nums = [1,3,4,2,2]
Output: 2
```

```
Input: nums = [3,1,3,4,2]
Output: 3
```

```
Input: nums = [3,3,3,3,3]
Output: 3
```

#### Constraints

* 1 <= n <= 10^5
* nums.length == n + 1
* 1 <= nums\[i] <= n
* All the integers in nums appear only once except for precisely one integer which appears two or more times.

Follow up:

* How can we prove that at least one duplicate number must exist in nums?
* Can you solve the problem in linear runtime complexity?

#### Explanation

From the description of the problem we learn that we have input of integers `nums` that contains `n + 1` integers. We also learn that we have only `one` repeated number that we need to return.

The brute force way to solve this problem would be to create a hash table and count the number of appearances of every single number and return the number that repeats twice or more times. The time and space complexity for this solution will be `O(n)`.

But we can't solve this problem in a brute force way because we are given constraints that tell us that we must solve this problem without extra space.

Another way to solve this problem is to use a linked list with cycle detection, and Floyd's algorithm which tells us the beginning of the cycle in the linked list.

### Hash Set Solution

```swift
func findDuplicate(_ nums: [Int]) -> Int {
    var seen: Set<Int> = []

    for num in nums {
        if seen.contains(num) {
            return num
        } else {
            seen.insert(num)
        }
    }

    return -1
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

### Fast and Slow Pointers Solution

```swift
func findDuplicate(_ nums: [Int]) -> Int {
    var slow = 0
    var fast = 0

    while true {
        slow = nums[slow]
        fast = nums[nums[fast]]
        if slow == fast {
            break
        }
    }

    var slow2 = 0
    while true {
        slow = nums[slow]
        slow2 = nums[slow2]

        if slow == slow2 {
            return slow
        }
    }
}
```

#### Explanation

Let's take a look at an example with input `[1, 3, 4, 2, 2]`.

We know that the length of our array is `n + 1` and every value in the array will be between `1` and `n`.
In our example we have `5` elements, and the range is from `1` to `4`.

Now let's make some changes. Instead of using our values, let's use our indices as pointers that will point at some position in our input.
![alt image](images/287.png#center)

You can see that we have a cycle, and that we can represent our indices as a linked list.
As for our `0` index, none of the other values will be pointing at index `0` because we have a range between `1` and `4`, so `0` is not going to be part of the cycle.

In the picture you can see that multiple nodes are pointing to node `2`.
Our next step will be identifying the beginning of the cycle in our linked list, and it will tell us that the beginning of the cycle is our return value. The way we are going to do it is by applying Floyd's algorithm.

Let's look at a slightly different example and visualize the work of Floyd's algorithm.
![alt image](images/287-1.png#center)

* The `fast` and `slow` pointers both start at position `0`.
* The `slow` pointer is going to be shifted by `one` each iteration.
  You can see in the picture that we can make `3` jumps using the `slow` pointer and `3` jumps using the `fast` pointer, and you can see that `3` is the first position where the two pointers intersect.

Finding the first intersection of two pointers was the first phase of Floyd's algorithm.

Our next step is to leave our `slow` pointer at the intersection. After that, we are going to create a `second slow` pointer and put it at the beginning of the array. Then we are going to shift both the `first slow` pointer and the `second pointer` one more time until they both intersect.
![alt image](images/287-2.png#center)

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
