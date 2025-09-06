+++
title = "LeetCode - 150 - Permutations"
date = 2025-09-06T07:42:44+03:00
tags = ["LeetCode", "150", "Permutations", "Swift"]
draft = false
+++

### The problem

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in any order.

> A permutation is a rearrangement of all the elements of an array.

#### Examples

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

```
Input: nums = [1]
Output: [[1]]
```

#### Constraints

* 1 <= nums.length <= 6
* -10 <= nums\[i] <= 10
* All the integers of nums are unique.

#### Explanation

From the description of the problem we learn that we need to return all possible permutations from an array of distinct integer values.

Let’s look at the example with input `nums = [1, 2, 3]` and try to understand how permutations will look.
![alt image](images/46.png#center)
We have three numbers therefore we have three positions to build:

* For the first position we have `three` choices, we can pick any element from our input
* For the second position we don’t know what element we picked before, but we know that we have only `two` elements remaining
* For the third position we only can pick `one` element

If we multiply `3*2*1` we will get `6` that tells us that in total we will have `six` different permutations for given input `[1, 2, 3]`.

To better understand how permutation will look let’s look at the decision tree of input `[1, 2, 3]`.
![alt image](images/46-1.png#center)
You can see that we have `six` permutations and every single one of them is different. We can solve this problem by using recursion but we will need to do a lot of bookkeeping because at each decision we need to add and remove elements.
![alt image](images/46-2.png#center)
We can choose a slightly easier approach by looking at it from a subproblems perspective.

### Recursion Solution

```swift
func permute(_ nums: [Int]) -> [[Int]] {
    let n = nums.count

    if n == 0 {
        return [[]]
    }

    let perms = permute(Array(nums[1..<n]))
    var res: [[Int]] = []

    for p in perms {
        for i in 0 ... p.count {
            var pCopy = p
            pCopy.insert(nums[0], at: i)
            res.append(pCopy)
        }
    }

    return res
}
```

#### Explanation

From the explanation above we learn that we can use subproblems to solve this problem.
![alt image](images/46-3.png#center)
Instead of branching our decision tree at the first step we can get all permutations from `1, 2, 3`:
* Next, the subproblem will be only with `2, 3` numbers
* Next, we will get permutations for the single number `3`
* Lastly, when we don’t have any elements we will return an `empty` array

Now, when we have our subproblems we can find our result by moving from the bottom up:
![alt image](images/46-4.png#center)

* If we don’t have any elements we will be returning up an `empty` array
* For the single value of `3` we can return only `one` permutation `[[3]]`
* For values `2, 3` we will be returning `two` permutations `[[2, 3], [3, 2]]` because at this point we have all permutations for value `3`, now we need to add value `2`. We know that we have two choices, we can put value `2` in the front of the array or we can put value `2` at the back.
* Lastly, for values `1, 2, 3` we are going to put value `1` in every position - we are going to try to put value `1` at the beginning, middle, and the end of the array, as a result we are going to end up with `three` different permutations.

In result for values `[2, 3]` we will have `[1, 2, 3], [2, 1, 3], [2, 3, 1]` permutations, and for values `[3, 2]` we will have `[1, 3, 2], [3, 1, 2], [3, 2, 1]` permutations, in total we will have `6` permutations.

#### Time/ Space complexity

* Time complexity: O(n!\*n^2)
* Space complexity: O(n!\*n)

#### Thank you for reading! 😊
