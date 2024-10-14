+++
title = 'DSA - Sorting - Merge Sort'
date = 2024-10-14T07:15:07+03:00
tags = ["DSA", "Data Structures", "Algorithms", "Sorting", "Merge Sort", "Swift"]
draft = false
+++

### What is Merge Sort?
[Merge sort](https://en.wikipedia.org/wiki/Merge_sort) is a recursive algorithm that uses the [divide and conquer](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm) algorithm design paradigm to find the solution.

![alt image](images/Merge-sort-example-300px.gif#center)
[source](https://en.wikipedia.org/wiki/Merge_sort#/media/File:Merge-sort-example-300px.gif)

The merge sort conceptually consists of two separate functions: `mergeSort` and `merge`. It works as follows:
- Divide the unsorted list into two equal halves.
- Recursively sort the two halves.
- [Merge](https://en.wikipedia.org/wiki/Merge_algorithm) the two halves to form a sorted array.

> There are multiple implementations of merge sort. I will be focusing on the [top-down implementation using lists](https://en.wikipedia.org/wiki/Merge_sort#Top-down_implementation_using_lists).

### Code Example
```swift
func mergeSort(_ array: [Int]) -> [Int] {
    if array.count < 2 {
        return array
    }
    let sortedLeftSide = mergeSort(Array(array[0 ..< array.count / 2]))
    let sortedRightSide = mergeSort(Array(array[array.count / 2 ..< array.count]))
    return merge(sortedLeftSide, sortedRightSide)
}

func merge(_ first: [Int], _ second: [Int]) -> [Int] {
    var result: [Int] = []
    var i = 0
    var j = 0
    while i < first.count && j < second.count {
        if first[i] <= second[j] {
            result.append(first[i])
            i += 1
        } else {
            result.append(second[j])
            j += 1
        }
    }
    while i < first.count {
        result.append(first[i])
        i += 1
    }
    while j < second.count {
        result.append(second[j])
        j += 1
    }
    return result
}
```

#### Explanation

##### `mergeSort` method:
- If the array’s `count` is less than 2, it means the array is already sorted and is returned as-is.
- Splits the array into two halves down the middle.
- Recursively calls the `mergeSort` method on the left side of the split array.
- Recursively calls the `mergeSort` method on the right side of the split array.
- Returns the result of the `merge` method with `sortedLeftSide` and `sortedRightSide` properties.

##### `merge` method:
- Creates a `result` list of integers.
- Sets `i` and `j` pointers to zero.
- Uses a loop to iterate over both the `first` and `second` input arrays. If an element in `first` is less than or equal to the respective element in `second`, it adds it to the final list and increments `i`. Otherwise, it adds the item from `second` to the final list and increments `j`.
- After comparing all the items, if there are any leftover elements in either `first` or `second`, it adds them to the `result`.


### Time Complexity
This algorithm has a time complexity of **O(n log n)**.

#### Thank you for reading! 😊