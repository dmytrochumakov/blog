+++
title = "LeetCode - Blind 75 - Insert Interval"
date = 2025-04-15T07:57:18+03:00
tags = ["LeetCode", "Blind 75", "Insert Interval", "Swift"]
draft = false
+++

### The problem  
You are given an array of non-overlapping intervals `intervals` where `intervals[i] = [starti, endi]` represents the start and the end of the `ith` interval, and `intervals` is sorted in ascending order by `starti`. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by `starti` and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

Note that you don't need to modify `intervals` in-place. You can make a new array and return it.

#### Examples

``` 
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
```

#### Constraints  
* 0 <= intervals.length <= 10^4  
* intervals[i].length == 2  
* 0 <= starti <= endi <= 10^5  
* intervals is sorted by starti in ascending order  
* newInterval.length == 2  
* 0 <= start <= end <= 10^5  

#### Explanation  
Let’s look at the example with input `intervals = [[1,3],[6,9]]`, newInterval = `[2,5]`.  
![alt image](images/57.png#center)

We can see that intervals `[1, 3]` and `[2, 5]` overlap with each other, so we are going to merge them. We are going to merge them by taking the minimum of both start values – `1`, and the maximum of both end values – `5`. Now the new interval will be `[1, 5]`.  
We also have one last interval `[6, 9]` that is not overlapping, so we will add that interval without any changes and the result will be `[[1,5],[6,9]]`.

One last thing before we jump into coding: in the description, we don’t have an explanation for the case where we have intervals like `[1,2]` and `[2,3]`.  
![alt image](images/57-1.png#center)  
In this case, the intervals are overlapping because the end of the first interval and the start of the second interval are connected. This counts as overlapping, so we need to merge them.

Let’s go through some simple cases with input `intervals = [[1,2],[3,4],[5,6]]`.  
![alt image](images/57-2.png#center)  
Let’s imagine we were given `newInterval = [-1, 0]`. Since the end value of this interval is less than the start value of the first interval, this means that this interval is not going to overlap with the first interval. Therefore, this interval is not going to overlap with any of the upcoming intervals either.

What if the opposite were true? Suppose we were given an interval `newInterval = [7, 8]` where the start value is greater than the end value of the last interval.  
![alt image](images/57-3.png#center)

In this case, the `newInterval` won’t overlap with any of the other intervals, so we can return the original list and add `newInterval` to the end of the list.

There also exist a few other possibilities:  
![alt image](images/57-4.png#center)  
- When `newInterval` overlaps with one of the other intervals and we need to combine both intervals  
- It is also possible that `newInterval` overlaps with multiple intervals—in this case, we need to combine multiple intervals  
- Or it could be that `newInterval` goes somewhere in between two intervals and does not overlap  

### Greedy Solution  
```swift
func insert(_ intervals: [[Int]], _ newInterval: [Int]) -> [[Int]] {
    let n = intervals.count
    var newInterval = newInterval
    var res: [[Int]] = []

    for i in 0 ..< n {
        if newInterval[1] < intervals[i][0] {
            res.append(newInterval)
            return res + intervals[i ..< n]
        } else if newInterval[0] > intervals[i][1] {
            res.append(intervals[i])
        } else {
            newInterval = [min(newInterval[0], intervals[i][0]), max(newInterval[1], intervals[i][1])]
        }
    }

    res.append(newInterval)

    return res
}
```

#### Explanation  
If we want to determine where `newInterval` goes, we have to iterate through the intervals and find an insertion point.  
- We are going to go interval by interval and check if the current interval overlaps with `newInterval`. If it doesn’t, we add the current interval to our result.  
To do that, we need to check:  
- If the end value of `newInterval` is less than the start value of the current interval  
- If the start value of `newInterval` is greater than the end value of the current interval  

If neither of these cases is true, this means that `newInterval` is overlapping and we need to merge them.  
We are going to merge them by taking the minimum of the start values and the maximum of the end values.

#### Time / Space complexity  
* Time complexity: O(n)  
* Space complexity: O(n)

#### Thank you for reading! 😊
