+++
title = "LeetCode - 150 - Search a 2D Matrix"
date = 2025-06-29T07:43:19+03:00
tags = ["LeetCode", "150", "Search a 2D Matrix", "Swift"]
draft = false
+++

### The problem

You are given an `m x n` integer matrix `matrix` with the following two properties:

* Each row is sorted in non-decreasing order.
* The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` if `target` is in `matrix` or `false` otherwise.

You must write a solution in `O(log(m * n))` time complexity.

#### Examples

![alt image](images/mat.jpg#center)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3  
Output: true  
```

![alt image](images/mat2.jpg#center)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13  
Output: false  
```

#### Constraints

* m == matrix.length
* n == matrix\[i].length
* 1 <= m, n <= 100
* -10^4 <= matrix\[i]\[j], target <= 10^4

#### Explanation

From the description of the problem, we learn that each row is sorted and the next row is going to be greater than the previous one. Therefore, each number throughout the matrix is going to be in sorted order.
The brute-force way to solve this problem is to search every single value in our input. This algorithm will take O(m \* n) time.

Let's look at another solution assuming that we just had a single row, and it was in sorted order.
![alt image](images/74.png#center)
We know that the most optimal algorithm that can search for a target would be binary search because we can do search in `O(logn)` time.

We can do it by running binary search on every single row until we find our `target`. This way we can satisfy the first property from our description. The time complexity for this solution will be `O(m * logn)`, but we can do better and optimize it even further.

We can optimize it further by following the second property. By knowing the fact that the next first row will be greater than the last integer in the previous row, we can apply binary search to determine in which row we can find our potential `target`.

If values in our current row are greater than our `target`, we will be crossing out the current row and will be looking at the row above the current row, because the top row will have smaller values than the bottom row.
![alt image](images/74-1.png#center)

This approach is more efficient because the first binary search will search for the row and will take `O(logm)` time, and the second binary search will search for the `target` value inside the row and will take `O(logn)` time. So the overall time complexity is `O(logm + logn)`.

### Binary Search Solution

```swift
func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {  
    let rows = matrix.count  
    let cols = matrix[0].count  

    var topRow = 0  
    var bottomRow = rows - 1  

    while topRow <= bottomRow {  
        let m = (bottomRow + topRow) / 2  
        if matrix[m].last! < target {  
            topRow = m + 1  
        } else if matrix[m][0] > target {  
            bottomRow = m - 1  
        } else {  
            break  
        }  
    }  

    let row = (bottomRow + topRow) / 2  
    var l = 0  
    var r = cols - 1  

    while l <= r {  
        let m = (l + r) / 2  
        if matrix[row][m] < target {  
            l = m + 1  
        } else if matrix[row][m] > target {  
            r = m - 1  
        } else {  
            return true  
        }  
    }  

    return false  
}  
```

#### Time/ Space complexity

* Time complexity:  O(logm + logn), which reduces to O(logm \* n)
* Space complexity: O(1)
* where `m` is the number of rows and `n` is the number of columns of the matrix

### Binary Search (One Pass) Solution

```swift
func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {  
    let rows = matrix.count  
    let cols = matrix[0].count  

    var l = 0  
    var r = rows * cols - 1  

    while l <= r {  
        let m = (l + r) / 2  
        let row = m / cols  
        let col = m % cols  

        if matrix[row][col] < target {  
            l = m + 1  
        } else if matrix[row][col] > target {  
            r = m - 1  
        } else {  
            return true  
        }  
    }  

    return false  
}  
```

#### Time/ Space complexity

* Time complexity:  O(logm \* n)
* Space complexity: O(1)
* where `m` is the number of rows and `n` is the number of columns of the matrix

#### Thank you for reading! 😊
