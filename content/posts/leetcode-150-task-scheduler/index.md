+++
title = "LeetCode - 150 - Task Scheduler"
date = 2025-08-19T07:34:45+03:00
tags = ["LeetCode", "150", "Task Scheduler", "Swift"]
draft = false
+++

### The problem

You are given an array of CPU `tasks`, each labeled with a letter from A to Z, and a number `n`. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of at least `n` intervals between two tasks with the same label.

Return the minimum number of CPU intervals required to complete all tasks.

#### Examples

```
Input: tasks = ["A","A","A","B","B","B"], n = 2

Output: 8

Explanation: A possible sequence is: A -> B -> idle -> A -> B -> idle -> A -> B.

After completing task A, you must wait two intervals before doing A again. The same applies to task B. In the 3rd interval, neither A nor B can be done, so you idle. By the 4th interval, you can do A again as 2 intervals have passed.
```

```
Input: tasks = ["A","C","A","B","D","B"], n = 1

Output: 6

Explanation: A possible sequence is: A -> B -> C -> D -> A -> B.

With a cooling interval of 1, you can repeat a task after just one other task.
```

```
Input: tasks = ["A","A","A", "B","B","B"], n = 3

Output: 10

Explanation: A possible sequence is: A -> B -> idle -> idle -> A -> B -> idle -> idle -> A -> B.

There are only two types of tasks, A and B, which need to be separated by 3 intervals. This leads to idling twice between repetitions of these tasks.
```

#### Constraints

* 1 <= tasks.length <= 10^4
* tasks\[i] is an uppercase English letter.
* 0 <= n <= 100

#### Explanation

To better understand the problem, let’s look at a visual representation of the first example.
We are given `tasks = ["A","A","A","B","B","B"]` and `n = 2`, and we need to find the minimum number of units of time to finish all of the tasks.

* If we processed one `A`, and after we try to process another `A` we will have to wait `n` units of time before we can do that.
  ![alt image](images/621.png#center)
* Instead of processing the second `A` we can process `B`, and remain idle `1` unit of time and then we can process our second `A` and our second `B`, because our idle time `n = 2`.
  ![alt image](images/621-1.png#center)
  As a result, we will return `8` in our output.

Let’s look at another example with input `AAA, BB, CC` and `n = 1`.

* The first thing that we need to do is to keep track of the count of each character.

Next, we will need to figure out what character we will be processing first:

* If we have chosen to process, for example, the `C` character first,
  ![alt image](images/621-2.png#center)
  we will end up adding idle time to wait until we process all `A`’s, and it will not be the minimum interval.

The more optimal way to process our characters is to start from the most frequent character, in our case it’s `A`. This way we can process different characters and minimize the idle time.
![alt image](images/621-3.png#center)

### Max Heap Solution

```swift
func leastInterval(_ tasks: [Character], _ n: Int) -> Int {
    var count: [Character: Int] = [:]
    for t in tasks {
        count[t, default: 0] += 1
    }
    var maxHeap = Heap(count.values)

    var time = 0
    var q: Deque<[Int]> = Deque()

    while !maxHeap.isEmpty || !q.isEmpty {
        time += 1
        if !maxHeap.isEmpty {
            let cnt = maxHeap.removeMax() - 1
            if cnt != 0 {
                q.append([cnt, time + n])
            }
        }

        if !q.isEmpty && q[0][1] == time {
            maxHeap.insert(q.removeFirst()[0])
        }
    }

    return time
}
```

#### Explanation

The optimal way to solve this problem is to use a max heap data structure. The max heap will help us determine what character is more frequent in `O(logn)` time.

We will be using the `time` property that will tell us the current time that we are at.

We will also be using a queue data structure to store our updated character count, and the time that will tell us when we can process this task again.
![alt image](images/621-4.png#center)
The steps of the algorithm will look like these:

* We are going to pop the max value from our max heap, decrease it by `1`, and add it to the queue as the first value.
* We are also going to calculate the time by getting our current time and adding `n` to it `(1 + 1 = 2)`. After that, we will add it to our queue as a second value.
* Next, we are going to increase our current time by `1`.
  ![alt image](images/621-5.png#center)
* Now, we are at time `2` and we can see that task `2, 2` is available to add back to our heap, so we are going to pop it from our queue and add it back to the heap.

Once time is `0` we are not going to do anything with it, because it does not have to be idle, so we are never going to add it back to our heap.

We are going to follow the same process until our heap or queue is not empty.
![alt image](images/621-6.png#center)

#### Time/ Space complexity

* Time complexity: O(m)
* Space complexity: O(1) since we have at most 26 different characters
* Where `m` is the number of tasks

#### Thank you for reading! 😊
