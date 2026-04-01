+++
title = "LeetCode - 150 - Distinct Subsequences"
date = 2026-04-01T08:04:07+03:00
tags = ["LeetCode", "150", "Distinct Subsequences", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

Given two strings `s` and `t`, return *the number of distinct* ***subsequences* of** `s` *which equals* `t`.

The test cases are generated so that the answer fits in a 32-bit signed integer.

**Example 1:**

```
Input: s = "rabbbit", t = "rabbit"
Output: 3
Explanation:
As shown below, there are 3 ways you can generate "rabbit" from s.
rabbbit
rabbbit
rabbbit
```

**Example 2:**

```
Input: s = "babgbag", t = "bag"
Output: 5
Explanation:
As shown below, there are 5 ways you can generate "bag" from s.
babgbag
babgbag
babgbag
babgbag
babgbag
```

**Constraints:**

* `1 <= s.length, t.length <= 1000`
* `s` and `t` consist of English letters.

### Dynamic Programming (Top-Down) Solution

#### Explanation

We are going to solve this problem using the DFS algorithm.
We will need two pointers `i` and `j` that correspond to characters from `s` and `t` to track subsequences.

If you look at the first example, you can see that the first four characters are equal. This means that we can increment both our pointers by **one**.
![alt image](images/115.png#center)

Now we are at the point where we no longer have matching characters, so we are going to increment the `i` pointer by **one** and leave the `j` pointer as it is because we are only looking for matching subsequences in `t`. For example:
![alt image](images/115-1.png#center)

Next, we are at the position where we have matching characters, therefore we are going to increment both pointers.
![alt image](images/115-2.png#center)

Next, our pointers are at the end of both strings where the characters are matching, therefore we are going to increment both pointers.
![alt image](images/115-3.png#center)

Lastly, we end up at the base case when our pointers are out of bounds.

At this point, we have only found **one** subsequence. In order to find the rest, we are going to add one more step to the matching statement where we don't skip a character in `t` and find the `total` from both recursive calls.
![alt image](images/115-4.png#center)

One more base case: if we have a string `s` that is **not empty** and a string `t` that **is empty**, we would be able to create **one** subsequence because to create an **empty** string we need to go out of bounds. For example:
![alt image](images/115-5.png#center)

On the other hand, if a string `t` is **not empty** and string `s` is **empty**, we would not be able to create a subsequence, therefore we would return **zero**.
![alt image](images/115-6.png#center)

```swift
func numDistinct(_ s: String, _ t: String) -> Int {
    let sArr = Array(s)
    let tArr = Array(t)
    let sLen = sArr.count
    let tLen = tArr.count

    if tLen > sLen {
        return 0
    }

    var cache = [[Int]: Int]()

    func dfs(_ i: Int, _ j: Int) -> Int {
        if j == tLen {
            return 1
        }

        if i == sLen {
            return 0
        }

        if let cached = cache[[i, j]] {
            return cached
        }

        var res = dfs(i + 1, j)

        if sArr[i] == tArr[j] {
            res += dfs(i + 1, j + 1)
        }

        cache[[i, j]] = res

        return res
    }

    return dfs(0, 0)
}
```

#### Time/ Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m*n)
* Where `m` is the length of string `s` and `n` is the length of string `t`

### Dynamic Programming (Bottom-Up) Solution

#### Explanation

Another way to solve this problem is to use the Bottom-Up DP approach. The core idea stays the same: when characters are equal, we increment our result.

The Bottom-Up approach is very close to the Top-Down approach, but instead of using a hashmap to cache our result, we use a 2D array with **one** additional row and column to calculate the base case when we have **empty** strings.

The last column is always going to be filled with **ones** because this means that we are at the base case when `t` is **empty** and we can create a subsequence.
![alt image](images/115-7.png#center)

After we handle the base case, we are going to start filling our `dp`. We follow the same principle as in the Top-Down approach, but instead of just putting a value into the cell `dp[i][j]`, we read the value from `dp[i + 1][j + 1]` and add it to the result in the cell.
![alt image](images/115-8.png#center)

code for the Bottom-Up approach is going to look like this:

```swift
func numDistinct(_ s: String, _ t: String) -> Int {
    let sArr = Array(s)
    let tArr = Array(t)

    let m = s.count
    let n = t.count
    var dp = Array(repeating: Array(repeating: 0, count: n + 1), count: m + 1)

    for i in 0 ..< (m + 1) {
        dp[i][n] = 1
    }

    for i in stride(from: m - 1, to: -1, by: -1) {
        for j in stride(from: n - 1, to: -1, by: -1) {
            let base = dp[i + 1][j]
            dp[i][j] = base
            if sArr[i] == tArr[j] {
                let addend = dp[i + 1][j + 1]
                if base > Int.max - addend {
                    dp[i][j] = 0
                } else {
                    dp[i][j] += addend
                }
            }
        }
    }

    return dp[0][0]
}
```

#### Time/ Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m*n)
* Where `m` is the length of string `s` and `n` is the length of string `t`

### Dynamic Programming (Space-Optimized) Solution

#### Explanation

The space-optimized solution is very similar to the Bottom-Up DP solution, but instead of using a **2D** array, it uses a **1D** array with only `O(n)` memory.

From the Bottom-Up solution, we know that `dp[i][j]` depends only on values from the next row `(i + 1)`. We don't use older rows, therefore we can use two **1D** arrays: the first to store values for the **next** row and the second to store values for the **current** row.

The rest of the algorithm stays the same: we can skip the current character in `s` or use it when it matches a character in `t`.

![alt image](images/115-9.png#center)

```swift
func numDistinct(_ s: String, _ t: String) -> Int {
    let sArr = Array(s)
    let tArr = Array(t)
    let sLen = sArr.count
    let tLen = tArr.count

    var dp = Array(repeating: 0, count: tLen + 1)
    var nextDP = Array(repeating: 0, count: tLen + 1)

    dp[tLen] = 1
    nextDP[tLen] = 1

    for i in stride(from: sLen - 1, to: -1, by: -1) {
        for j in stride(from: tLen - 1, to: -1, by: -1) {

            nextDP[j] = dp[j]

            if sArr[i] == tArr[j] {
                let addend = dp[j + 1]
                if nextDP[j] > Int.max - addend {
                    nextDP[j] = 0
                } else {
                    nextDP[j] += addend
                }
            }

        }

        dp = nextDP
    }

    return dp[0]
}
```

#### Time/ Space complexity

* Time complexity: O(m*n)
* Space complexity: O(n)
* Where `m` is the length of string `s` and `n` is the length of string `t`

#### Thank you for reading! 😊
