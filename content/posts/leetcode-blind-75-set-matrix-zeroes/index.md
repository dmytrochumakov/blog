+++
title = "LeetCode - Blind 75 - Set Matrix Zeroes"
date = 2025-05-02T07:58:01+03:00
tags = ["LeetCode", "Blind 75", "Set Matrix Zeroes", "Swift"]
draft = false
+++

### The problem  
Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

#### Examples  
![alt image](images/mat1.jpg#center)  
```
Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]  
Output: [[1,0,1],[0,0,0],[1,0,1]]  
```

![alt image](images/mat2.jpg#center)  
```
Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]  
Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]  
```

#### Constraints  
* m == matrix.length  
* n == matrix[0].length  
* 1 <= m, n <= 200  
* -2³¹ <= matrix[i][j] <= 2³¹ - 1

Follow up:  
* A straightforward solution using O(m\*n) space is probably a bad idea.  
* A simple improvement uses O(m + n) space, but still not the best solution.  
* Could you devise a constant space solution?

### Brute Force Solution  
```swift
func setZeroes(_ matrix: inout [[Int]]) {
    let rows = matrix.count
    let cols = matrix[0].count
    var mark: [[Int]] = []

    for r in 0 ..< rows {
        var row: [Int] = []
        for c in 0 ..< cols {
            row.append(matrix[r][c])
        }
        mark.append(row)
    }

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if matrix[r][c] == 0 {
                for col in 0 ..< cols {
                    mark[r][col] = 0
                }
                for row in 0 ..< rows {
                    mark[row][c] = 0
                }
            }
        }
    }

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            matrix[r][c] = mark[r][c]
        }
    }
}
```

#### Explanation  
![alt image](images/73.png#center)

We can solve this problem by declaring a `copy` of our input array.  
The reason why we can’t do it `in-place` is because when we replace `1` in the last column with `0`, we end up making the entire column filled with `0`s, breaking the correct order.  
This is why we use `copy`: when we make changes, we make them to our `copy` without breaking anything, and when we read, we read from our `input` array.

In the example  
![alt image](images/73-1.png#center)  
you can see that when we encounter `0`, we are going to update the entire first row, and also update the entire column in our `copy` array.  
We created `copy` because when we go to the next position and we see that it’s `1`, we do not modify the column, because we have not read that value yet.

Next, we continue moving to the next position in our matrix and when we encounter `0`, we set the entire row to `0`, but in our `copy` we can see that we already did it for the middle value.

Lastly, we go to our last position, and we can see that it is `0`, so we are going to set the entire column to `0`, and we are going to set the entire row to `0` in our `copy`.

You can see that in this solution we have a lot of repetitive work; therefore, we can prevent this from happening.

#### Time/ Space complexity  
* Time complexity: O((m\*n) \* (m+n))  
* Space complexity: O(m \* n)  
* where `m` is the number of rows, and `n` is the number of columns

### Iteration Solution  
```swift
func setZeroes(_ matrix: inout [[Int]]) {
    let ROWS = matrix.count
    let COLS = matrix[0].count
    var rows = Array(repeating: false, count: ROWS)
    var cols = Array(repeating: false, count: COLS)

    for r in 0 ..< ROWS {
        for c in 0 ..< COLS {
            if matrix[r][c] == 0 {
                rows[r] = true
                cols[c] = true
            }
        }
    }

    for r in 0 ..< ROWS {
        for c in 0 ..< COLS {
            if rows[r] || cols[c] {
                matrix[r][c] = 0
            }
        }
    }
}
```

#### Explanation  
We can improve the brute force solution’s time and space complexity by only allocating additional row and column arrays to keep track of what we need to update.  
![alt image](images/73-2.png#center)

We start from our first row, and when we encounter `0`, we mark our additional row and column.  
After that, we move to the next row, and when we see `0` in it, we mark that row, but we do not mark the column because we already did it.  
Lastly, we move to our last row and mark the row and column that need to be set to `0`.

Now, we look at the marked rows and columns, and set them to `0`.  
![alt image](images/73-3.png#center)

The advantage of this algorithm is that the memory used is O(m+n), and the time complexity is O(m\*n).

We can go even further and optimize the memory complexity.

#### Time/ Space complexity  
* Time complexity: O(m\*n)  
* Space complexity: O(m+n)  
* where `m` is the number of rows, and `n` is the number of columns

### Iteration (Space Optimized) Solution  
```swift
func setZeroes(_ matrix: inout [[Int]]) {
    let rows = matrix.count
    let cols = matrix[0].count
    var rowZero = false

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if matrix[r][c] == 0 {
                matrix[0][c] = 0
                if r > 0 {
                    matrix[r][0] = 0
                } else {
                    rowZero = true
                }
            }
        }
    }

    for r in 1 ..< rows {
        for c in 1 ..< cols {
            if matrix[0][c] == 0 || matrix[r][0] == 0 {
                matrix[r][c] = 0
            }
        }
    }

    if matrix[0][0] == 0 {
        for r in 0 ..< rows {
            matrix[r][0] = 0
        }
    }

    if rowZero {
        for c in 0 ..< cols {
            matrix[0][c] = 0
        }
    }
}
```

#### Explanation  
![alt image](images/73-4.png#center)

We can take our additional row and column from the previous solution and put them inside our matrix so that we can do our algorithm **in-place**, but we are going to need one additional variable.  
If you look at the top-left corner, you can see that the row and column overlap at that place.

The rest of the algorithm stays the same as in the previous solution.  
To sum up:  
- We are going to keep track of `rowZero`.  
- Set our column to `0` when we encounter value `0`.  
- If we encounter `0` in the middle of the matrix, we are going to set the cell in the first row with the current column to `0`,  
- and also set `0` to the cell in the first column with the current row position.  

![alt image](images/73-5.png#center)

This way, we can keep track of which rows and columns we should zero out by looking at the values in the first row and column, and moving from top-left and working our way down — from top to bottom, from left to right.

#### Time/ Space complexity  
* Time complexity: O(m \* n)  
* Space complexity: O(1)  
* where `m` is the number of rows, and `n` is the number of columns  

#### Thank you for reading! 😊
