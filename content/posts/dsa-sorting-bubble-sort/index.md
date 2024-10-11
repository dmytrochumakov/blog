+++
title = 'DSA - Sorting - Bubble Sort'
date = 2024-10-11T07:02:51+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Sorting", "Bubble Sort", "Swift"]
draft = false
+++

### What is the Bubble Sort Algorithm?
The [bubble sort](https://en.wikipedia.org/wiki/Bubble_sort) is a basic sorting algorithm named after the way elements progressively "bubble up" to the top of the list.

![alt image](images/Bubble-sort-example-300px.gif#center)
[source](https://en.wikipedia.org/wiki/Bubble_sort#/media/File:Bubble-sort-example-300px.gif)

### Code Example
```swift
func bubbleSort(_ array: [Int]) -> [Int] {
    var A = array
    var N = array.count
    var swapping = true
    while swapping {
        swapping = false
        for i in 1 ..< N {
            if A[i - 1] > A[i] {
                let tmp = A[i - 1]
                A[i - 1] = A[i]
                A[i] = tmp
                swapping = true
            }
        }
        N -= 1
    }
    return A
}
```

#### Implementation
The bubble sort uses an loop and `swapping` property to control it behaviour. Inside the loop, it iterates over all elements, comparing the current and next elements. If the current element is greater than the next, it swaps them.

The example above uses [optimized bubble sort](https://en.wikipedia.org/wiki/Bubble_sort#:~:text=swapped%0Aend%20procedure-,Optimizing%20bubble%20sort,-%5Bedit%5D), where the inner loop avoids looking at the last n − 1 items when running for the n-th time, resulting in about a 50% improvement in the worst-case scenario.

### Time/Space Complexity
Bubble sort has a worst-case and average time complexity of O(n²), where n is the number of items being sorted.  
The worst-case space complexity is O(n) total, O(1) auxiliary.

#### Thank you for reading! 😊
