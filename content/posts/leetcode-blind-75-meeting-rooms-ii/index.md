+++
title = "LeetCode - Blind 75 - Meeting Rooms II"
date = 2025-04-25T07:25:34+03:00
tags = ["LeetCode", "Blind 75", "Meeting Rooms II", "Swift"]
draft = false
+++

### The problem  
Given an array of meeting time intervals `intervals` where `intervals[i] = [starti, endi]`, return the minimum number of conference rooms required.

#### Examples

``` 
Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
```

```
Input: intervals = [[7,10],[2,4]]
Output: 1
```

#### Constraints  
* 1 <= intervals.length <= 10^4  
* 0 <= starti < endi <= 10^6

#### Explanation  
Before jumping to the code, we are going to sort the input because it's not sorted by default.

### Sorting Solution  
```swift 
func minMeetingRooms(_ intervals: [[Int]]) -> Int {
    let start = intervals.map({ $0[0] }).sorted()
    let end = intervals.map({ $0[1] }).sorted()
    var res = 0
    var count = 0
    var s = 0
    var e = 0
    let n = intervals.count

    while s < n {
        if start[s] < end[e] {
            s += 1
            count += 1
        } else {
            e += 1
            count -= 1
        }
        res = max(res, count)
    }

    return res
}
```

#### Explanation  
We can visualize input with `intervals = [[0,30],[5,10],[15,20]]` like this:  
![alt image](images/253.png#center)

- If we go from left to right, we are going to see the `first` meeting start at time `0`.  
- We keep going and the next meeting `started` at time `5`, and it tells us that we have `two` meetings that have started but no meeting that has ended.

For the counting process, we will be creating and maintaining a `count` property that will count the number of meetings going on.

- At time `10`, we can see that the second meeting has ended, therefore we are going to decrement our `count` to `1`.  
- Now, we are going to look to the next point in order, where another meeting has `started` at `15`, and we will increment our `count` property, which will be equal to `2`.  
- Next, we are going to repeat the same process and take the next point where the meeting has `ended` at `20`. After that, we are going to take our `count` and decrement it by 1 — now `count == 1`.  
- Lastly, we are going to go to our last point, which is also an `end` time. That means another meeting is stopping, so we decrement our `count`, therefore `count == 0`.

We noticed that the `max` `count` that happened was `2`, so we are going to return `2` as our result.

Now let's look at how we are going to sort these intervals and how we are going to iterate over the meetings regardless of whether it's a `start` time for a meeting or its `end` time.  
Input `intervals = [[0,30],[5,10],[10,15]]` looks like this:  
![alt image](images/253-1.png#center)

Look at the time where point time equals `10`: we have two intervals — one that `ends` at `10` and another that `starts` at `10`. That means these meetings are not overlapping.  
If we ever have a tie (two points with the exact same value), we always iterate through the `end` meeting time before we iterate through the `start` meeting time.

We are going to have two input arrays, `start` and `end`. After that, we are going to start this problem off using two pointers:

- We are going to have one pointer at the beginning of the `start` array, and another at the beginning of the `end` array.  
- Between the `start` value and `end` value, we are going to peek at the `minimum`. If the `minimum` of the two is a `start` point, then we are going to increment our `count` by `1` and shift the `start` pointer to the next value. We will continue doing this until we reach a tie.  
- When we reach the tie, the meeting has to end, so we will shift our `end` pointer to the next one.  
- If we iterate through an `end` value, that means the meeting has ended, therefore we are going to decrement our `count` by `1`, `count == 1`.  
- Now, we are going to compare values `10` and `15`, where value `10` is smaller, and we are going to increment our `count` by `1`, `count == 2`.  
- Now we don't have any `start` times left, so we are going to iterate through `end` times.

From the solution above, we can see that our `max` `count` was equal to `2`. Therefore, in the result, we will be returning `2`.

#### Time/Space Complexity  
* Time complexity: O(n*logn)  
* Space complexity: O(n)

#### Thank you for reading! 😊
