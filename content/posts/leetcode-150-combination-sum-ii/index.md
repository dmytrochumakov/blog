+++
title = "LeetCode - 150 - Combination Sum II"
date = 2025-09-02T07:47:08+03:00
tags = ["LeetCode", "150", "Combination Sum II", "Swift"]
draft = false
+++

### The problem

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sum to `target`.

Each number in `candidates` may only be used once in the combination.

> The solution set must not contain duplicate combinations.

#### Examples

```
Input: candidates = [10,1,2,7,6,1,5], target = 8
Output: 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

```
Input: candidates = [2,5,2,1,2], target = 5
Output: 
[
[1,2,2],
[5]
]
```

#### Constraints

* 1 <= candidates.length <= 100
* 1 <= candidates\[i] <= 50
* 1 <= target <= 30

#### Explanation

From the description of the problem, we learn that we are given an array of numbers `candidates` and we want to find every combination that will sum to the given `target`.

Let's look at our example with `candidates = [10,1,2,7,6,1,5], target = 8`
![alt image](images/40.png#center)
You can see that we can choose values that will sum up to `target = 8`:

* `1, 1, 6`
* `1, 2, 5`
* `1, 7`
* `2, 6`

Technically we could possibly have chosen values `7, 1` and `6, 2`, but they count as the same combinations. In the problem statement, they mention that we can’t have any duplicate values, therefore we can’t add values `7, 1` and `6, 2` to our result.

Another limitation that we have is that we can't use the same value an unlimited amount of times, for example, we can't use `1` eight times, but we can use a value that shows up multiple times in our array.

To avoid the scenario with adding duplicate values, we will be sorting our input array. This will help us order values so that all duplicate values will be grouped together.
![alt image](images/40-1.png#center)

We will have index `i` that will help us track at which position we are currently at, and we will be updating `i` until its neighbor value is not a duplicate value.

### Backtracking Solution

```swift
func combinationSum2(_ candidates: [Int], _ target: Int) -> [[Int]] {
    var candidates = candidates
    candidates.sort()
    let n = candidates.count
    var res: [[Int]] = []

    func dfs(_ i: Int, _ curr: inout [Int], _ total: Int) {
        if total == target {
            res.append(curr)
            return
        }
        if total > target {
            return
        }
        if i >= n {
            return
        }

        curr.append(candidates[i])
        dfs(i + 1, &curr, total + candidates[i])

        curr.removeLast()
        var nextIndex = i + 1
        while nextIndex < n && candidates[i] == candidates[nextIndex] {
            nextIndex += 1
        }
        dfs(nextIndex, &curr, total)
    }

    var curr: [Int] = []
    dfs(0, &curr, 0)
    return res
}
```

#### Explanation

To solve this problem we will be using a depth-first search algorithm and a while loop that will help us avoid duplicate values in the result.

Let's look at an example of how our decision tree would look:
![alt image](images/40-2.png#center)

* When we are at the first element, we are going to decide whether to add `1` or `empty array`.
* Next, we could decide whether to add another `1` or skip it.
* Next, we can try to add `2`, and after that `5`, but we can’t because `total sum = 1 + 1 + 2 + 5 = 9`, which is more than our `target = 8`, so we can stop our recursion and skip this path without including any values from it in our result (this will be one of our base cases).
* Next, we are going to decide to include value `6` and add `[1, 1, 6]` to our result because the total sum equals `target = 8`.

I will skip explaining all of the possible decisions because it's a very long decision tree and will focus only on values that are equal to `target`.

* Next, you can imagine that we skipped all values that do not sum to the `target`, so we can decide to add value `7` and add `[1, 7]` to our result.
* Next, we will add `[1, 2, 5]` to our result.
* Lastly, we would decide to add `[2, 6]` to our result.

#### Time/ Space complexity

* Time complexity: O(n\*2^n)
* Space complexity: O(n)

#### Thank you for reading! 😊
