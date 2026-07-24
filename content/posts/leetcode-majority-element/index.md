+++
title = "LeetCode - Majority Element"
date = 2026-07-24T09:38:49+03:00
tags = ["LeetCode", "Majority Element", "Arrays_&_Hashing", "Swift"]
draft = false
+++

### The problem

Given an array `nums` of size `n`, return *the majority element*.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

```
Input: nums = [3,2,3]
Output: 3
```

**Example 2:**

```
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

**Constraints:**

* `n == nums.length`

* `1 <= n <= 5 * 10^4`

* `-10^9 <= nums[i] <= 10^9`

* The input is generated such that a majority element will exist in the array.

**Follow-up:** Could you solve the problem in linear time and in `O(1)` space?

#### Explanation

The simple way to solve this problem is to find the size of the array, count all elements using a HashMap, and find the element that appears more than `n/2` times. This solution will take `O(n)` time and space complexity. The trick is in the follow-up section, where we should solve this problem in linear time and `O(1)` space.

### Boyer-Moore Majority Voting Solution

#### Explanation

We can satisfy our follow-up requirements by using the Boyer-Moore Majority Vote algorithm.

The core idea of this algorithm lies in finding the majority element in a sequence of items. The only requirement is that the majority element must appear more than half the time (> n/2 times) in the dataset.

In our case, we are guaranteed that the majority element always exists; therefore, we can easily apply this algorithm to our problem.

The way this algorithm works is that we start scanning through the array, and instead of allocating memory for a count HashMap, we only track two variables:

* *result* - the majority element
* *count* - the number of occurrences of the same element

During the iteration, we check if *result* is equal to the current value:

* if it does, we increment the *count* variable
  ![alt image](images/169.png#center)
* if it does not, we decrement *count*
  ![alt image](images/169-1.png#center)
* Lastly, if *count* is equal to **zero**, we are going to update **result** to the current value.
  ![alt image](images/169-2.png#center)

At the end of execution, our **result** has the correct answer.
![alt image](images/169-3.png#center)

#### Code

```swift
func majorityElement(_ nums: [Int]) -> Int {
    var res = 0
    var count = 0

    for num in nums {
        if count == 0 {
            res = num
        }

        if num == res {
            count += 1
        } else {
            count -= 1
        }
    }

    return res
}
```

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(1)

#### Thank you for reading! 😊
