+++
title = "LeetCode - Blind 75 - Non-overlapping Intervals"
date = 2025-04-20T07:59:44+03:00
tags = ["LeetCode", "Blind 75", "Non-overlapping Intervals", "Swift"]
draft = false
+++

### The problem  
Given an array of `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Note that intervals which only touch at a point are non-overlapping. For example, `[1, 2]` and `[2, 3]` are non-overlapping.

#### Examples

```
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

```
Input: intervals = [[1,2],[1,2],[1,2]]
Output: 2
Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
```

```
Input: intervals = [[1,2],[2,3]]
Output: 0
Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
```

#### Constraints
* 1 <= intervals.length <= 10^5  
* intervals[i].length == 2  
* -5 * 10^4 <= starti < endi <= 5 * 10^4  

#### Explanation  
Before we dive into coding, let’s figure out the definition of overlapping intervals for this problem.  
![alt image](images/435.png#center)

- The intervals with values `[1, 2]` and `[3, 4]` are not considered overlapping  
- Next, the intervals with values `[1, 3]` and `[2, 4]` are considered overlapping  
- Lastly, the intervals with values `[1,2]` and `[2,3]` are not considered overlapping. If they have the same edge point, they are not counted as overlapping.

### Recursion Solution  
```swift 
func eraseOverlapIntervals(_ intervals: [[Int]]) -> Int {
    var intervals = intervals
    intervals.sort(by: { $0[0] <  $1[0] })

    let n = intervals.count

    func dfs(_ i: Int, _ prev: Int) -> Int {
        if i == n {
            return 0
        }

        var res = dfs(i + 1, prev)

        if prev == -1 || intervals[prev][1] <= intervals[i][0] {
            res = max(res, 1 + dfs(i + 1, i))
        }

        return res
    }

    return n - dfs(0, -1)
}
```

#### Explanation  
We need to visualize the problem to be able to solve it. Let’s look at the first example with `intervals = [[1,2],[2,3],[3,4],[1,3]]`  
![alt image](images/435-1.png#center)

We can see that intervals `[1,2],[2,3],[3,4]` are not overlapping, but when you insert interval `[1,3]` they become overlapping.  
Now we want to eliminate intervals, and we want to eliminate the `minimum` number:  
- We can remove only interval `[1, 3]`, but it's not the only way we could do it  
- We could remove intervals `[1,2]` and `[2,3]`, but that's not what we are looking for  

The brute force way to solve this problem is to go through every single combination, where we can choose to remove an interval or keep an interval.  
If we have two choices for every single interval inside a list of intervals, the time complexity to check every single possibility would be O(2^n). That is not very efficient and we can do better.

#### Time/ Space complexity  
* Time complexity: O(2^n)  
* Space complexity: O(n)  

### Dynamic Programming Top-Down Solution  
```swift 
func eraseOverlapIntervals(_ intervals: [[Int]]) -> Int {
    var intervals = intervals
    intervals.sort(by: { $0[1] <  $1[1] })

    let n = intervals.count
    var memo: [Int: Int] = [:]

    func dfs(_ i: Int) -> Int {
        if memo[i] != nil {
            return memo[i]!
        }

        var res = 1

        for j in  i + 1 ..< n {
            if intervals[i][1] <= intervals[j][0] {
                res = max(res, 1 + dfs(j))
                break
            }
        }

        memo[i] = res

        return res
    }

    return n - dfs(0)
}
```  

#### Explanation  
In the previous solution, we learned that we can optimize the recursive solution. We can do it by caching it.

The way this works:  
- We would add an additional hash map where we would store index and the number of intervals that we need to remove as value, so this way we would eliminate repeated work and improve our time complexity.

We can go even further and get O(n) time complexity.

#### Time/ Space complexity  
* Time complexity: O(n^2)  
* Space complexity: O(n)  

### Greedy Sort By Start Solution  
```swift 
func eraseOverlapIntervals(_ intervals: [[Int]]) -> Int {
    let intervals = intervals.sorted(by: { $0[0] < $1[0] })
    var res = 0
    var prevEnd = intervals[0][1]

    for interval in intervals.dropFirst() {
        let start = interval[0]
        let end = interval[1]

        if start >= prevEnd {
            prevEnd = end
        } else {
            res += 1
        }
        prevEnd = min(prevEnd, end)
    }

    return res
}
```  

#### Explanation  
Before we dive into the solution, we need to remember that the order of intervals that we are given can be random, therefore our first step would be to sort our input by start position.  
![alt image](images/435-2.png#center)

- After that, we are going to iterate through the sorted input and compare adjacent pairs of intervals  

Next, we would check if intervals are overlapping by comparing the `end` of the first interval and the `start` of the second interval  
- If the second interval `starts` after the first one `ends`, then they are not overlapping  
- But if the second interval `starts` before the first one `ends`, then they are overlapping  

Lastly, we are going to remove the interval that ends first  

#### Time/ Space complexity  
* Time complexity: O(n)  
* Space complexity: O(n)

#### Thank you for reading! 😊
