+++
title = "LeetCode - 150 - Subsets"
date = 2025-08-27T08:00:51+03:00
tags = ["LeetCode", "150", "Subsets", "Swift"]
draft = false
+++

### The problem

Given an integer array `nums` of unique elements, return all possible subsets (the power set).

> A subset of an array is a selection of elements (possibly none) of the array.

The solution set must not contain duplicate subsets. Return the solution in any order.

#### Examples

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

```
Input: nums = [0]
Output: [[],[0]]
```

#### Constraints

* 1 <= nums.length <= 10
* -10 <= nums\[i] <= 10
* All the numbers of nums are unique.

#### Explanation

From the description of the problem we learn that we need to return every single subset that we can create from input `nums`, without any duplicates.

If we look at the example with `nums = [1, 2, 3]`
![alt image](images/78.png#center)
we can create subsets `[1]`, `[2]`, `[1, 2]` etc.

* If we, for example, swapped the order of subset `[1, 2]` to `[2, 1]` it can't be added to the result because it’s a duplicate,
* The input `[1, 2, 3]` counts as a subset
* The empty subset also counts

When we are creating subsets for a given input we need to make a choice for every single element.
For the input `nums = [1, 2, 3]`:

* We can include `1` - `[1]` or not include `1`, and add the empty set `[]`
  ![alt image](images/78-1.png#center)

### Backtracking Solution

```swift
func subsets(_ nums: [Int]) -> [[Int]] {
    var res: [[Int]] = []
    var subset: [Int] = []
    let n = nums.count

    func dfs(_ i: Int) {
        if i >= n {
            res.append(subset)
            return
        }
        
        // decision to include nums[i]
        subset.append(nums[i])
        dfs(i + 1)

        // decision NOT to include nums[i]
        subset.removeLast()
        dfs(i + 1)
    }

    dfs(0)

    return res
}
```

#### Explanation
Let’s draw a decision tree to better understand how we are going to solve this problem.
![alt image](images/78-2.png#center)

* The first decision that we are going to make is to add or not `1`
* Next, we are going to decide whether to add `2` or not
* Next, on the right side of the tree we can decide whether to add `2` or not
* Lastly, we want to decide whether to add `3` or not

As a result, you can see that we have built unique subsets.

Let’s take a look at the algorithm steps:

* For backtracking we will be using depth-first search algorithm
* We will be passing `i` which will tell us the index of the value that we are making a decision on
* Next, we will need to take care of the base case and check if our index `i` is not out of bounds
* Next, we will be tracking our elements into `subset`, and when we reach our base case we will add our `subset` to `res`
* Lastly, we are going to have two decisions - decision to include `nums[i]` and decision not to include `nums[i]`. To do that we will append `nums[i]` to our `subset` and recursively call `dfs` on the next index, then remove the last element, and recursively call `dfs` on the next index

#### Time/ Space complexity

* Time complexity: O(n\*2^n)
* Space complexity: O(n) extra space, O(2^n) for output array.

#### Thank you for reading! 😊
