+++
title = 'DSA - Sorting - Selection Sort'
date = 2024-10-23T07:19:52+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Sorting", "Selection Sort", "Swift"]
draft = false
+++

### Introduction
The [selection sort](https://en.wikipedia.org/wiki/Selection_sort) is an in-place comparison sorting algorithm. It's similar to bubble sort in that it works by repeatedly swapping items in a list and not very efficient on larger lists.

![alt image](images/Selection-Sort-Animation.gif#center)
[source](https://en.wikipedia.org/wiki/Selection_sort#/media/File:Selection-Sort-Animation.gif)

### Code Example
```swift
func selectionSort(_ array: [Int]) -> [Int] {
    var A = array
    let N = array.count

    for i in 0 ..< N {
        var jMin = i
        for j in (i + 1) ..< N {
            if A[j] < A[jMin] {
                jMin = j
            }
        }
        if jMin != i {
            let tmp = A[i]
            A[i] = A[jMin]
            A[jMin] = tmp
        }
    }

    return A
}
``` 

#### Implementation
For each index:  
- Set `jMin` index to the current index  
- For each index from `i + 1` to the end of the list:  
  - If the element at inner index `j` is less than the element at index `jMin`, set `jMin` to `j`  
- If `jMin` does not equal `i`, swap the element at the current index `i` with the element at index `jMin`

### Time/Space Complexity
Time complexity: O(N^2)  
Space complexity: O(1) [auxiliary](https://en.wikipedia.org/wiki/Computer_data_storage#Secondary_storage)

#### Thank you for reading! 😊
