+++
title = "LeetCode - 150 - Subsets II"
date = 2025-09-09T07:18:32+03:00
tags = ["LeetCode", "150", "Subsets II", "Swift"]
draft = false
+++

### The problem

Given an integer array `nums` that may contain duplicates, return all possible subsets (the power set).

> A subset of an array is a selection of elements (possibly none) of the array.

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Examples

```
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

```
Input: nums = [0]
Output: [[],[0]]
```

#### Constraints

* 1 <= nums.length <= 10
* -10 <= nums\[i] <= 10

#### Explanation

From the description of the problem we learn that we are given `nums` that contains duplicate values and we need to return subsets without any duplicates.
Let's look at an example with input `[1, 2, 2]`
![alt image](images/90.png#center)
From the given input:

* we can create a subset with a single value `1` - `[1]`
* we can also create a subset with a single value `2` - `[2]`
* when we reach the last value `2` we can't create a subset for it because we already have a subset with that value
* `empty` also counts as a valid subset
* next, we can take values `1` and `2` that will give us subset - `[1, 2]`
* we can't take value `1` and the second `2` because it will create a duplicate

As you can see we need to find a way to not include duplicates while we are creating subsets. We can do that by using a backtracking algorithm.

### Backtracking Solution

```swift
func subsetsWithDup(_ nums: [Int]) -> [[Int]] {
    var nums = nums
    var res: [[Int]] = []
    nums.sort()

    let n = nums.count
    var subset: [Int] = []

    func backtrack(_ i: Int) {
        if i == n {
            res.append(subset)
            return
        }

        subset.append(nums[i])
        backtrack(i + 1)
        subset.removeLast()

        var skipIdx: Int = i
        while skipIdx + 1 < n && nums[skipIdx] == nums[skipIdx + 1] {
            skipIdx += 1
        }
        backtrack(skipIdx + 1)
    }

    backtrack(0)

    return res
}
```

#### Explanation

Above we learn that we can solve this problem by using a backtracking algorithm.

Let's look at the visual representation of the decision tree with a slightly different example `[1, 2, 2, 3]`:
![alt image](images/90-1.png#center)

In our decision tree:

* At the first step we are going to decide to include or not include value `1`
* Next, we are at the second position so we can decide either to add value `2` or skip it, the same logic applies to the `empty` array - we can add value `2` or skip it
* At the next position we can decide to add our second value `2`, but we can't do it because at this point we have two subsets that are the same and the remainder for both of them will be the same so we are going to end up with duplicate subsets

When we are creating a subset starting from value `1` we are making a decision to include value `2`, to prevent duplicates from happening we can include value `2` only once and move our `i` pointer to the `3` skipping the second value `2`. To do that we will need to sort our input so the same values will be close to each other.
![alt image](images/90-2.png#center)

#### Time/ Space complexity

* Time complexity: O(n\*2^n)
* Space complexity: O(n) extra space and O(2^n) for the output space

#### Thank you for reading! 😊
