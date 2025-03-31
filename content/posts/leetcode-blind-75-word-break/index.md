+++
title = "LeetCode - Blind 75 - Word Break"
date = 2025-03-31T07:40:53+03:00
tags = ["LeetCode", "Blind 75", "Word Break", "Swift"]
draft = false
+++

### The problem 
Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

#### Examples

``` 
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```

#### Constraints
* 1 <= s.length <= 300
* 1 <= wordDict.length <= 1000
* 1 <= wordDict[i].length <= 20
* `s` and `wordDict[i]` consist of only lowercase English letters.
* All the strings of `wordDict` are unique.

### Recursion solution 
```swift 
func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
    let sCount = s.count
    let sArray = Array(s)

    func dfs(_ i: Int) -> Bool {
        if i == sCount {
            return true
        }

        for w in wordDict {
            let wCount = w.count
            if ((i + wCount) <= sCount &&
                String(sArray[i ..< i + wCount]) == w
            ) {
                if dfs(i + wCount) {
                    return true
                }
            }
        }
        return false
    }

    return dfs(0)
}
```

#### Explanation
There are multiple ways to solve this problem. For example, we can check every character in input `s` and compare it with characters in `wordDict`, but this approach is inefficient from a time complexity perspective. Instead: 
- we can take an entire word from `wordDict`, 
- find its length, 
- get a substring with this length from input `s`, 
- and compare it. 

This approach is much more efficient because the max size of `wordDict` is smaller than the max size of input `s`.

Let's start by visualizing our problem using a decision tree with input `s = “neetcode”`, `wordDict = ["neet”, “leet”, “code”]`.

![alt image](images/139.png#center)

We are going to keep track of index `i`. Our decision tree will have three decisions: "neet", "leet", "code".
- We check the first `4` characters in `s = “neetcode”` and compare them. If they match “neet”, “leet”, or “code”, we proceed; otherwise, we stop that path.
- "neet" does match, so we create three more decisions, one for each word in `wordDict`. When we move to the “neet” word, our `i` pointer becomes `4`.
- After that, we move our pointer and repeat the previous step. If “code” matches one of our options, we update our `i` pointer by adding `4`, making `i = 8`. Since this is the end of the string, we return `true`.

This is not a very efficient solution because it takes a lot of time, but we can optimize it using caching.

#### Time/Space Complexity
* Time complexity:  O(t * m^n)
* Space complexity:  O(n)
* where `n` is the length of `s`, `m` is the number of words in `wordDict`, and `t` is the maximum length of any word in `wordDict`.

### Dynamic Programming Top-Down Solution  
```swift 
func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
    let sCount = s.count
    let sArray = Array(s)
    var memo: [Int: Bool] = [sCount : true]

    func dfs(_ i: Int) -> Bool {
        if memo[i] != nil {
            return memo[i]!
        }

        for w in wordDict {
            let wCount = w.count
            if ((i + wCount) <= sCount &&
                String(sArray[i ..< i + wCount]) == w
            ) {
                if dfs(i + wCount) {
                    memo[i] = true
                    return true
                }
            }
        }
        memo[i] = false
        return false
    }

    return dfs(0)
}
``` 

#### Explanation
From the previous solution, we learned that using recursion without caching results in a slow algorithm because the decision tree can grow exponentially depending on `wordDict` input size and the length of `s`. To avoid repetitive work at the same index `i`, we use a hash map to store the result. If we ever reach the same index `i` again, we return the cached result immediately.

#### Time/Space Complexity
* Time complexity:  O(n * m * t)
* Space complexity:  O(n)
* where `n` is the length of `s`, `m` is the number of words in `wordDict`, and `t` is the maximum length of any word in `wordDict`.

### Dynamic Programming Bottom-Up Solution  
```swift 
func wordBreak(_ s: String, _ wordDict: [String]) -> Bool {
    let sCount = s.count
    let sArray = Array(s)

    var dp = Array(repeating: false, count: sCount + 1)
    dp[sCount] = true

    for i in stride(from: sCount - 1, to: -1 , by: -1) {
        for w in wordDict {
            let wCount = w.count
            if ((i + wCount) <= sCount && String(sArray[i ..< i + wCount]) == w) {
                dp[i] = dp[i + wCount]
            }
            if dp[i] {
                break
            }
        }
    }

    return dp[0]
}
``` 

#### Explanation
We discovered that our base case will be `dp[8]`, because our input `s = “neetcode”` has a length of `8`. When we reach the last index, we return `true`.  
![alt image](images/139-1.png#center)

Now we apply a bottom-up approach.

We iterate through every index in reverse order:
- At `dp[7]`, `dp[6]`, and `dp[5]`, we get `false` since there are not enough characters after these indices to form a valid word from `wordDict`.
- At `dp[4]`, we set `dp[4] = true` because it matches "code".
- At `dp[3]`, `dp[2]`, and `dp[1]`, we get `false` because no words in `wordDict` start with these characters.
- Finally, at `dp[0]`, we match "neet" from `wordDict`. Since we previously computed `dp[4] = true`, we set `dp[0] = true`.

#### Time/Space Complexity
* Time complexity:  O(n * m * t)
* Space complexity:  O(n)
* where `n` is the length of `s`, `m` is the number of words in `wordDict`, and `t` is the maximum length of any word in `wordDict`.

#### Thank you for reading! 😊
