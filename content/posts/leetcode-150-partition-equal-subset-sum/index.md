+++
title = "LeetCode - 150 - Partition Equal Subset Sum"
date = 2026-01-24T09:59:57+03:00
tags = ["LeetCode", "150", "Partition Equal Subset Sum", "1D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

Given an integer array `nums`, return `true` if you can partition the array into two subsets such that the sum of the elements in both subsets is equal, or `false` otherwise.

#### Examples

```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
```

#### Constraints

* `1 <= nums.length <= 200`
* `1 <= nums[i] <= 100`

#### Explanation

From the description of the problem, we learn that we are given a non-empty array `nums` that contains only positive integers, and we want to find out if we can partition the array into two different subsets such that the sum of each subset is equal.

This means that we can take one subset of the array whose sum is equal to **half of the sum** of the entire array.
For example, in the first input we have `nums = [1, 5, 11, 5]`
![alt image](images/416.png#center)
The sum of all numbers equals `22`, and half of the sum equals `11`.
You can see that we can choose a subset with a single value `[11]`, and we can also choose numbers with values `[1, 5, 5]` that sum up to `11`.

Now, when we have a basic understanding of the problem, let's try to figure out what a brute-force solution would look like.

We can start solving this problem by building a decision tree:
![alt image](images/416-1.png#center)

* We can choose to add the value `1` or skip it.
* Next, we are going to add the value `5` or skip it.
* Next, we are going to try to add the value `11`.

You can see in the picture that we found our target value `11`, so we don't need to continue anymore. We can skip the last value and return our result.

The brute-force solution is not very efficient because it will take `O(2^n)` time.

### Dynamic Programming Solution

```swift
func canPartition(_ nums: [Int]) -> Bool {
    let total = nums.reduce(0, +) 

    if total % 2 == 1 {
        return false
    }

    var dp: Set<Int> = []
    dp.insert(0)

    let target = total / 2

    for i in stride(from: nums.count - 1, to: -1, by: -1) {
        for t in dp {
            if (t + nums[i]) == target {
                return true
            }
            dp.insert(t + nums[i])
            dp.insert(t)
        }
    }

    return dp.contains(target)
}
```

#### Explanation

To optimize time complexity, we can use a dynamic programming technique.
![alt image](images/416-2.png#center)
We are going to iterate over our input backwards. We are going to have a hash set where we will be storing the amounts of sums that we can create for our current number.

We are going to have **two** possible choices: we are going to **take** or **skip** the current number.

* We are going to start with the value `5` and add two elements to our hash set: `0` and `5`.
  ![alt image](images/416-3.png#center)
* Next, we are going to move to the value `11`. We are going to take that value and add it to the previous elements in our hash set, and then we are going to add the results `11` and `16` to the hash set.
  ![alt image](images/416-4.png#center)
* Next, we are going to take the value `5` and add it to all values in our hash set. As a result, we will get values `10` and `21`. We are going to skip values that are already in our hash set, because a hash set can only store unique values.
  ![alt image](images/416-5.png#center)
* Lastly, we are going to take the value `1`, repeat the same steps as we did before, and as a result we will get values `1, 6, 12, 17, and 22`.

When we complete the iteration, we are going to check if our hash set contains the **target** value. If it does, we are going to return `true`; otherwise, we return `false`.

> We need to take care of a corner case: if the **sum** of our input `nums` is **odd**, it would be impossible to partition the input into equal halves, therefore we return `false`.

#### Time/ Space complexity

* Time complexity: `O(n * target)`
* Space complexity: `O(target)`
* Where `n` is the number of items in `nums`, and `target` is the sum of `nums` divided by 2.

#### Thank you for reading! 😊
