+++
title = "LeetCode - Blind 75 - Merge Intervals"
date = 2025-04-18T07:23:08+03:00
tags = ["LeetCode", "Blind 75", "Merge Intervals", "Swift"]
draft = false
+++

### The problem  
Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

#### Examples

``` 
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

#### Constraints  
* 1 <= intervals.length <= 10^4  
* intervals[i].length == 2  
* 0 <= starti <= endi <= 10^4  

#### Explanation  
Let's look at examples and figure out our base cases for intervals that are considered overlapping.

In the first example, we are given `intervals = [[1,3],[2,6],[8,10],[15,18]]`,  
![alt image](images/56.png#center)

and we can see that the pairs `[[1,3],[2,6]]` are overlapping.

In the second example, they are telling us that `intervals = [[1,4],[4,5]]` are also considered overlapping.  
![alt image](images/56-1.png#center)

### Sorting Solution  
``` swift 
func merge(_ intervals: [[Int]]) -> [[Int]] {
    var intervals = intervals
    intervals.sort(by: { $0[0] < $1[0] })
    var output = [intervals[0]]

    for interval in intervals.dropFirst() {
        let start = interval[0]
        let end = interval[1]

        var lastEnd = output[output.count - 1][1]
        if start <= lastEnd {
            output[output.count - 1][1] = max(lastEnd, end)
        } else {
            output.append([start, end])
        }
    }

    return output
}
```

#### Explanation  
Now let's say that we were given the first example but not in sorted order: `intervals = [[1,3],[8,10],[15,18],[2,6]]`  
![alt image](images/56-2.png#center)

When we draw a number line, we can see that we can take the intervals and sort them based on the start value. That way, we would be able to determine if intervals are overlapping, and if they do, we can merge them into a new interval.

The first step in solving this problem is:  
- to sort our intervals by start value  
- next, iterate through every single interval in sorted order — we can skip the first value because we added it to our `output`
- after that, we are going to compare the `lastEnd` value with our `start` value, and if the `start` value is less than or equal to `lastEnd`, we will merge intervals by finding the `max` end value  
- if values are not overlapping, we will be adding the interval to our `output`  

#### Time/ Space complexity  
* Time complexity: O(n * logn)  
* Space complexity: O(n)

#### Thank you for reading! 😊
