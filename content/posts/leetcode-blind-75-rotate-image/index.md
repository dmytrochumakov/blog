+++
title = "LeetCode - Blind 75 - Rotate Image"
date = 2025-04-28T07:13:43+03:00
tags = ["LeetCode", "Blind 75", "Rotate Image", "Swift"]
draft = false
+++

### The problem 
You are given an `n x n` 2D `matrix` representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image [in-place](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

#### Examples
![alt image](images/mat1.jpg#center)
``` 
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

![alt image](images/mat2.jpg#center)
```
Input: matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
Output: [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
```

#### Constraints
* n == matrix.length == matrix[i].length
* 1 <= n <= 20
* -1000 <= matrix[i][j] <= 1000

#### Explanation
Before we jump to the solution, let's look at how rotation was done to the `matrix = [[1,2,3],[4,5,6],[7,8,9]]`.
![alt image](images/48.png#center)

The problem occurs when you move value `1` to the position of value `3`, because you have to save value `3` temporarily. The same logic applies to values `9` and `7`.
We also have other values in our outer layer that we need to take care of, values `4`, `2`, `6`, `8`.
During rotation:
- value `2` is going to replace value `6`
- value `6` is going to replace value `8`
- value `8` is going to replace value `4`
- value `4` is going to replace value `2`

Lastly, we have the left value `5`, that has size 1 by 1, and we can not rotate it.

### Solution 
``` swift 
func rotate(_ matrix: inout [[Int]]) {
    let n = matrix.count
    var l = 0
    var r = n - 1

    while l < r {
        for i in 0 ..< r - l {
            let top = l
            let bottom = r
            let topLeft = matrix[top][l + i]

            matrix[top][l + i] = matrix[bottom - i][l]
            matrix[bottom - i][l] = matrix[bottom][r - i]
            matrix[bottom][r - i] = matrix[top + i][r]
            matrix[top + i][r] = topLeft
        }
        r -= 1
        l += 1
    }
}
```

#### Explanation
Let's look at another example.
![alt image](images/48-1.png#center)

We are going to start rotating from the outermost square layer. After that, we are going to move inward, and we are going to do rotation inside the matrix.
The rotation is going to start from `top` `left` and we will go clockwise.
- When we did the outermost layer, now we need to do the inside layer.
- We can treat the inside of the matrix as a subproblem, all we need to do is to shift our pointers by `1`.

As for in-memory replacement, we are going to keep our value in a temporary variable and replace it with the next value.
We can slightly improve our solution with temporary variables by doing it in reverse order. Instead of moving values clockwise, we will do it counter-clockwise. The reason we do this is because we only need one temporary variable, which will make the code a little bit easier.

#### Time/Space complexity
* Time complexity: O(n^2)
* Space complexity: O(1)

#### Thank you for reading! 😊
