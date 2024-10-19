+++
title = 'DSA - Sorting - Quick Sort'
date = 2024-10-19T06:51:04+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Sorting", "Quick Sort", "Swift"]
draft = false
+++

### Introduction
The [quick sort](https://en.wikipedia.org/wiki/Quicksort) is an efficient sorting algorithm commonly used widely in production. Quick sort is a [divide-and-conquer algorithm](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm). It works by selecting a pivot from the array and partitioning the other elements into two subarrays.

![alt image](images/Sorting_quicksort_anim.gif#center)
[source](https://en.wikipedia.org/wiki/Quicksort#/media/File:Sorting_quicksort_anim.gif)

### Code example 
```swift 
// Sorts a (portion of an) array, divides it into partitions, then sorts those
func quickSort(array: inout [Int], low: Int, high: Int) {
    // Ensure indices are in correct order
    if low < high {

        // Partition array and get the pivot index
        let p = partition(array: &array, low: low, high: high)

        // Sort the two partitions
        quickSort(array: &array, low: low, high: p - 1) // Left side of pivot
        quickSort(array: &array, low: p + 1, high: high) // Right side of pivot
    }
}

// Divides array into two partitions
func partition(array: inout [Int], low: Int, high: Int) -> Int {
    let pivot = array[high] // Choose the last element as the pivot

    // Temporary pivot index
    var i = low

    for j in low ..< high {
        // If the current element is less than or equal to the pivot
        if array[j] <= pivot {
            // Swap the current element with the element at the temporary pivot index
            let tmp = array[i]
            array[i] = array[j]
            array[j] = tmp
            // Move the temporary pivot index forward
            i += 1
        }
    }

    // Swap the pivot with the last element
    let tmp = array[i]
    array[i] = array[high]
    array[high] = tmp

    return i
}
```

#### Implementation
##### quickSort
1. Ensure that the `low` and `high` indices are in the correct order.
2. Partition the input list using the `partition` function.
3. Recursively call `quickSort` on the left side of the pivot.
4. Recursively call `quickSort` on the right side of the pivot.

##### partition
1. Choose the last element as the pivot.
2. Set `i` to `low`.
3. For each `j` from `low` to `high` (not inclusive), compare if the current element is less than or equal to the pivot:  
   3.1 Swap the element at index `i` with the element at index `j`.  
   3.2 Increment `i` by `1`.
4. Swap the element at index `i` with the last element.
5. Return `i`.

### Time/Space complexity
Time complexity: O(N^2)  
Space complexity: O(N)

#### Thank you for reading! 😊
