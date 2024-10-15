+++
title = 'DSA - Sorting - Insertion Sort'
date = 2024-10-15T07:16:04+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Sorting", "Insertion Sort", "Swift"]
draft = false
+++

### Introduction 
The [insertion sort](https://en.wikipedia.org/wiki/Insertion_sort) algorithm builds a sorted list one item at a time [by comparison](https://en.wikipedia.org/wiki/Comparison_sort). It is more efficient on small datasets but much less efficient on larger ones.

![alt image](images/Insertion_sort.gif#center)
[source](https://en.wikipedia.org/wiki/Insertion_sort#/media/File:Insertion_sort.gif)

![alt image](images/Insertion-sort-example-300px.gif#center)
[source](https://en.wikipedia.org/wiki/Insertion_sort#/media/File:Insertion-sort-example-300px.gif)

### Code Example 
```swift 
func insertionSort(_ array: [Int]) -> [Int] {
    var A = array
    for i in 0 ..< A.count {
        var j = i
        while j > 0 && A[j - 1] > A[j] {
            let tmp = A[j]
            A[j] = A[j - 1]
            A[j - 1] = tmp
            j -= 1
        }
    }
    return A
}
```

#### Implementation 
1. For each index in the input array:
   1. Set the `j` variable to `i`. 
2. Loop while `j` is greater than `0` and the element at index `j - 1` is greater than the element at index `j`:  
   1. Swap the elements at index `j` and `j - 1`.  
   2. Decrement `j` by 1.

### Time/Space Complexity 
- Time: O(N^2)  
- Space: O(1) (in-place)

#### Thank you for reading! 😊
