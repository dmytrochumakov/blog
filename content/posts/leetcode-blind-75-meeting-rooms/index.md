+++
title = "LeetCode - Blind 75 - Meeting Rooms"
date = 2025-04-22T07:25:26+03:00
tags = ["LeetCode", "Blind 75", "Meeting Rooms", "Swift"]
draft = false
+++

### The problem  
Given an array of meeting time `intervals` where `intervals[i] = [starti, endi]`, determine if a person could attend all meetings.

#### Examples

``` 
Input: intervals = [[0,30],[5,10],[15,20]]
Output: false
```

```
Input: intervals = [[7,10],[2,4]]
Output: true
```

#### Constraints
* 0 <= intervals.length <= 10^4  
* intervals[i].length == 2  
* 0 <= starti < endi <= 10^6

#### Explanation  
Before we dive into coding, let's look at base cases in which intervals are not overlapping.  
![alt image](images/252.png#center)

Intervals `[0, 8]` and `[8, 10]` are not considered overlapping, so if one interval `starts` at `8` and the second interval `ends` at `8`, this means that these intervals are not overlapping, and if we encounter similar cases we would return `true`.

### Sorting Solution  
``` swift 
func canAttendMeetings(_ intervals: [[Int]]) -> Bool {
    if intervals.isEmpty {
        return true
    }

    let sortedIntervals = intervals.sorted(by: { $0[0] < $1[0] })
    let n = sortedIntervals.count

    for i in 1 ..< n {
        let i1 = sortedIntervals[i - 1]
        let i2 = sortedIntervals[i]
        if i1[1] > i2[0] {
            return false
        }
    }

    return true
}
```

#### Explanation  
Now, let's look at another example with `intervals = [[0,30],[5,10],[15,20]]`  
![alt image](images/252-1.png#center)

- We can see that the first meeting `starts` at 0 and `ends` at 30,  
- The second interval `starts` at 5 but it `ends` at 10, and you may notice that this meeting `starts` before the first meeting `ends`, meaning that the first interval and second interval are overlapping and we have to return `false`.

You can see that sorting will be helpful to us to solve this problem. We will be sorting all meetings based on the `start` time of each meeting.

- The first part of the algorithm will be sorting  
- The second part will be scanning through the entire array of intervals  

![alt image](images/252-2.png#center)

We are going to look at the first two intervals and compare the `end` time of the first interval and the `start` time of the second interval.  
- If the `start` time of the second interval is before the `end` time of the first interval, that means they are overlapping.  
- If the second interval `starts` where the first interval `ends`, that does not mean that intervals are overlapping.  

We do not need to compare the `first` interval and the `third` interval because we sorted the input, and we know that the `second` interval does not overlap with the `first` interval. Therefore, there is no way that the `third` interval could possibly overlap with the `first` interval.

Now when we move to the position of the `second` interval, we are going to be checking if the `second` and `third` intervals are overlapping, and we are going to repeat the process described above again by comparing `start` and `end` time of intervals.

#### Time/ Space complexity  
* Time complexity: O(n*logn)  
* Space complexity: O(n)

#### Thank you for reading! 😊
