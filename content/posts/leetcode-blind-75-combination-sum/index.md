+++
title = "LeetCode - Blind 75 - Combination Sum"
date = 2025-02-05T14:11:48+03:00
tags = ["LeetCode", "Blind 75", "Combination Sum", "Swift"]
draft = false
+++

### The Problem  
Given an array of **distinct** integers `candidates` and a target integer `target`, return a list of all **unique combinations** of `candidates` where the chosen numbers sum to `target`. You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited** number of times. Two combinations are unique if the **frequency** of at least one of the chosen numbers is different.

> The **frequency** of an element `x` is the number of times it occurs in the array.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` for the given input.

---

#### Examples  

```swift
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

```swift
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

```swift
Input: candidates = [2], target = 1
Output: []
```

#### Constraints  
* `1 <= candidates.length <= 30`  
* `2 <= candidates[i] <= 40`  
* All elements of `candidates` are **distinct**.  
* `1 <= target <= 40`  

### Backtracking Solution  

```swift
func combinationSum(_ candidates: [Int], _ target: Int) -> [[Int]] {
    let n = candidates.count
    var res: [[Int]] = []
    var curr: [Int] = []

    func dfs(_ i: Int, _ total: Int) {
        if total == target {
            res.append(curr)
            return
        }

        if i >= n || total > target {
            return
        }

        curr.append(candidates[i])
        dfs(i, total + candidates[i])
        curr.removeLast()
        dfs(i + 1, total)
    }

    dfs(0, 0)

    return res
}
```

#### Explanation  
When a problem requires finding **unique combinations** of elements, it usually means the solution involves a **backtracking algorithm**. Before diving into the code, the best way to understand the problem is to **draw the decision tree**.  

For example, with the input `[2,3,6,7]` and `target = 7`, we **cannot** continue with `7` because adding it to any other value in the array would exceed the `target`.  
![alt image](images/p-39-1.png#center)  

One of the **requirements** is that we **cannot** have duplicate values. To enforce this, we construct a decision tree where:  
- One branch **includes** all possible occurrences of `2`.  
- The other branch **skips** `2` to avoid duplicates.  

We achieve this by **adding and removing** elements from the array and using **recursion**.  
![alt image](images/p-39-2.png#center)  

#### Time/Space Complexity  
* **Time Complexity:** O(2^{t/m}) 
* **Space Complexity:** O(t/m)
  *Where `t` is the given `target` and `m` is the minimum value in `candidates`.*  

### Optimal Backtracking Solution  

```swift
func combinationSum(_ candidates: [Int], _ target: Int) -> [[Int]] {
    var candidates = candidates
    let n = candidates.count
    var res: [[Int]] = []
    var curr: [Int] = []

    candidates.sort()

    func dfs(_ i: Int, _ total: Int) {
        if total == target {
            res.append(curr)
            return
        }

        for j in i ..< n {
            if total + candidates[j] > target {
                return
            }
            curr.append(candidates[j])
            dfs(j, total + candidates[j])
            curr.removeLast()
        }
    }

    dfs(0, 0)

    return res
}
```

#### Explanation  
We can **slightly optimize** the first solution by **sorting** the elements.  
- This allows us to **find results faster** if we have a small `target` value.  
- We **reduce the recursion call stack** by adding an additional iteration.  

#### Time/Space Complexity  
* **Time Complexity:** O(2^{t/m}) 
* **Space Complexity:** O(t/m) 
  *Where `t` is the given `target` and `m` is the minimum value in `candidates`.*  

#### Thank you for reading! 😊
