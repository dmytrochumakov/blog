+++
title = "LeetCode - 150 - Edit Distance"
date = 2026-03-18T08:15:54+03:00
tags = ["LeetCode", "150", "Edit Distance", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

LeetCode - 150 - Edit Distance

### The problem

Given two strings `word1` and `word2`, return *the minimum number of operations required to convert `word1` to `word2`*.

You have the following three operations permitted on a word:

* Insert a character

* Delete a character

* Replace a character

**Example 1:**

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

**Example 2:**

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```

**Constraints:**

* `0 <= word1.length, word2.length <= 500`
* `word1` and `word2` consist of lowercase English letters.

### Recursion Solution

#### Explanation

With the problem description in mind, let's go through the base cases that we can get from our constraints:
![alt image](images/72.png#center)

* one of the base cases is when both `word1` and `word2` are empty strings. In this case, it would take `0` operations to convert `word1` to `word2`.
  ![alt image](images/72-1.png#center)
* the next base case would be if we had the same strings in `word1` and `word2`, for example strings like `abc`. In this case, it would also take `0` operations to convert `word1` to `word2`.
  ![alt image](images/72-2.png#center)
* the next case would be if we had characters in `word1` but an empty string in `word2`. For example `word1 = "abc"`, `word2 = ""`. In this case, we would need to **remove** all characters from `word1` and return the result `3`.
  ![alt image](images/72-3.png#center)
* in the case where we have an empty string in `word1` but characters in `word2`, for example `word1 = ""`, `word2 = "abc"`, we would need to **insert** all characters from `word2` into `word1`, which would result in `3` operations.

> if one of `word1` or `word2` is empty, we would return the length of the **non-empty** string.

![alt image](images/72-4.png#center)
when both `word1` and `word2` are not empty, we are going to compare them character by character. For example, let's imagine that we have `word1 = "acd"` and `word2 = "abd"`. We are also going to have pointers `i` and `j` that will help us track the current character in each word.

![alt image](images/72-5.png#center)

* if you look at the example, you can see that the first characters in both words are equal, therefore we are going to increment both pointers `i` and `j` by one: `(i + 1, j + 1)`. We did not perform any operations here, therefore the operations count stays `0`.

![alt image](images/72-6.png#center)
when we move to the next characters, you can see that they are not equal. This means that we can apply `insert`, `delete`, `replace` operations:
![alt image](images/72-7.png#center)

* if we try to `insert` character `b` into `word1`, we would need to increase the operations count by one, increment the `j` pointer by one, and leave the `i` pointer in the same position.
  ![alt image](images/72-8.png#center)
* if we try to `delete` the `c` character in `word1`, we would need to increase the operations count, increment the `i` pointer, and leave the `j` pointer in the same position.
  ![alt image](images/72-9.png#center)
* lastly, if we try to `replace` the `c` character in `word1` with the `b` character from `word2` so they match, we would need to increase the operations count and increment both `i` and `j` pointers.

Now that we know all base cases, we can apply that knowledge and develop an algorithm that is going to solve this problem.

```swift
func minDistance(_ word1: String, _ word2: String) -> Int {
    let m = word1.count
    let n = word2.count
    let word1Arr = Array(word1)
    let word2Arr = Array(word2)

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == m {
            return n - j
        }

        if j == n {
            return m - i
        }

        if word1Arr[i] == word2Arr[j] {
            return dfs(i + 1, j + 1)
        }

        var res = min(dfs(i + 1, j), dfs(i, j + 1))
        res = min(res, dfs(i + 1, j + 1))

        return res + 1
    }

    return dfs(0, 0)
}
```

#### Time/ Space complexity

* Time complexity: O(3^(m + n))
* Space complexity: O(m + n)
* Where `m` is the length of `word1` and `n` is the length of `word2`

### Dynamic Programming (Top-Down) Solution

#### Explanation

If you look at the time complexity of the recursive solution, you can see that it is not very efficient. Furthermore, it won't pass on LeetCode. When we are using a recursive solution, we visit the same `i, j` positions multiple times, so to avoid that we are going to use caching that will store already calculated results in a dictionary.

```swift
func minDistance(_ word1: String, _ word2: String) -> Int {
    let m = word1.count
    let n = word2.count
    let word1Arr = Array(word1)
    let word2Arr = Array(word2)

    struct Key: Hashable {
        let i: Int
        let j: Int
    }
    var cache: [Key: Int] = [:]

    func dfs(_ i: Int, _ j: Int) -> Int {
        if i == m {
            return n - j
        }

        if j == n {
            return m - i
        }

        if let cached = cache[Key(i: i, j: j)] {
            return cached
        }

        if word1Arr[i] == word2Arr[j] {
            cache[Key(i: i, j: j)] = dfs(i + 1, j + 1)
        } else {
            var res = min(dfs(i + 1, j), dfs(i, j + 1))
            res = min(res, dfs(i + 1, j + 1))
            cache[Key(i: i, j: j)] = res + 1
        }

        return cache[Key(i: i, j: j)]!
    }

    return dfs(0, 0)
}
```

#### Time/ Space complexity

* Time complexity: O(m * n)
* Space complexity: O(m * n)
* Where `m` is the length of `word1` and `n` is the length of `word2`

### Dynamic Programming (Bottom-Up) Solution

#### Explanation

So far in our solutions, we used recursion, but in order to solve this problem in a true dynamic programming way, we are going to create a 2D grid where we will store the **minimum number of operations to make substrings equal**. We are also going to add an additional row and column to our grid—this is going to help us work with the base case when we have an empty string in one or both of our words.
![alt image](images/72-10.png#center)

let's look at base cases:

* If both words are empty, it will take `0` operations because both strings are equal.
  ![alt image](images/72-11.png#center)
* If we were calculating the operations count for a column with an **empty string**, we would calculate the length of `word1` at the current row and put the value in the current cell. For example, for the first row and the last column, it will take `word1.count = 3` operations; for the second row `2`, etc.
  ![alt image](images/72-12.png#center)
* The same logic applies for the last row, but instead of calculating `word1` count, we are going to calculate `word2` count. For example, for the first row and first column, the operations count is going to be `word2.count = 3`, for the second column `2`, etc.
  ![alt image](images/72-13.png#center)

Since we have **three** choices where we can move—to the right, below, and diagonally—we are going to iterate from bottom up and find the **minimum number of operations** for each cell. Then, when the iteration is complete, we are going to return the result from the first row and column.

For example, for row `d` and column `d`, we are going to put `0` in the cell because they are both equal and it took us `0` operations. For the row `d` and column `c`, even though our minimum is going to be `0`, we are going to add `1` to it because those values are not equal.
![alt image](images/72-14.png#center)

```swift
func minDistance(_ word1: String, _ word2: String) -> Int {
    let word1Arr = Array(word1)
    let word2Arr = Array(word2)
    var dp = Array(repeating: Array(repeating: Int.max, count: word2.count + 1), count: word1.count + 1)

    for i in 0 ..< word1.count + 1 {
        dp[i][word2.count] = word1.count - i
    }

    for j in 0 ..< word2.count + 1 {
        dp[word1.count][j] = word2.count - j
    }

    for i in stride(from: word1.count - 1, to: -1, by: -1) {
        for j in stride(from: word2.count - 1, to: -1, by: -1) {
            if word1Arr[i] == word2Arr[j] {
                dp[i][j] = dp[i + 1][j + 1]
            } else {
                dp[i][j] = 1 + min(dp[i + 1][j], dp[i][j + 1], dp[i + 1][j + 1])
            }
        }
    }

    return dp[0][0]
}
```

#### Time/ Space complexity

* Time complexity: O(m * n)
* Space complexity: O(m * n)
* Where `m` is the length of `word1` and `n` is the length of `word2`

#### Thank you for reading! 😊
