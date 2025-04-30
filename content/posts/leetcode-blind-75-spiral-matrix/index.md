+++
title = "LeetCode - Blind 75 - Spiral Matrix"
date = 2025-04-30T07:27:55+03:00
tags = ["LeetCode", "Blind 75", "Spiral Matrix", "Swift"]
draft = false
+++

### The problem 
Given an `m x n` `matrix`, return all elements of the `matrix` in spiral order.

#### Examples
![alt image](images/spiral1.jpg#center)
``` 
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [1,2,3,6,9,8,7,4,5]
```

![alt image](images/spiral.jpg#center)
```
Input: matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

#### Constraints
* m == matrix.length  
* n == matrix[i].length  
* 1 <= m, n <= 10  
* -100 <= matrix[i][j] <= 100

#### Explanation
Let's look at the example with `matrix = [[1,2,3],[4,5,6],[7,8,9]]` and try to figure out a way to solve this problem.  
![alt image](images/54.png#center)

We are going to start from the top left corner:  
- Go right from `1` to `2`,  
- Go right again from `2` to `3`, now we can’t go right anymore  
- Next, we go down from `3` to `6`,  
- Go down one more time from `6` to `9`,  
- Now we are going to go left from `9` to `8` because we can’t go down anymore,  
- Go left again from `8` to `7`  
- We can’t go left anymore, so we go up from `7` to `4`, and if we try to go up, we see that we already visited it and we can’t go there  
- Lastly, we go right from `4` to `5`. If we try to go right, we can see that we cannot go right anymore because we already visited that element  

![alt image](images/54-1.png#center)

To solve this problem we are going to start from the top left corner:  
- Go right until we visit all of them  
- Next, we go down and visit all available elements  
- Next, we go left and visit all of the elements  
- Next, we go up  

We went through all four directions, but we still have some elements left.  
- We took the outermost layer and shrank it. Now we have a sub-matrix, and we can do the same algorithm on it.  
- We can do it by introducing `left` and `right` boundaries that we move inward by `1`. 
- Similarly, we have `top` and `bottom` boundaries that we also need to shrink by `1`.  
- After that, we are going to solve the sub-matrix in spiral order by going from `top` `left` to the `right`.  
- You can notice that we’ve gone through all elements, so now we can move our `left` and `right` boundaries once more, and stop because we don’t have a rectangle anymore and we visited all elements.

### Iteration Solution 
```swift
func spiralOrder(_ matrix: [[Int]]) -> [Int] {
    var res: [Int] = []
    var l = 0
    var r = matrix[0].count
    var top = 0
    var bottom = matrix.count

    while l < r && top < bottom {
        for i in l ..< r {
            res.append(matrix[top][i])
        }
        top += 1

        for i in top ..< bottom {
            res.append(matrix[i][r - 1])
        }
        r -= 1

        if !(l < r && top < bottom) {
            break
        }

        for i in stride(from: r - 1, to: l - 1, by: -1) {
            res.append(matrix[bottom - 1][i])
        }
        bottom -= 1

        for i in stride(from: bottom - 1, to: top - 1, by: -1) {
            res.append(matrix[i][l])
        }
        l += 1
    }

    return res
}
```

#### Explanation
![alt image](images/54-2.png#center)

We are going to initialize the `left` boundary at `0`, `right` at `len(matrix[0])`, `top` at `0`, and `bottom` at `len(matrix)` to solve this problem in an easier way and avoid edge cases.  
We are always going to start at the `top` `left` position.  
- After that, we are going to go `right` and iterate through the first row, adding values to our result until we reach our `right` boundary.  
- Now we can go `down`, so we need to shift our `top` boundary down by `1`, add values to our result, and continue going down until we reach our `bottom` boundary  
- We can see that we visited all elements in the last column, therefore we can now move our `right` boundary by decreasing it by `1`  
- Next, we can go through elements to the `left` until we reach our `left` boundary and add those elements to our result  
- When we reach the `left` boundary, you can see that we also finished the entire `bottom` row, meaning that we can shift our `bottom` boundary up by `1`  
- Next, we are going to go `up` until we reach our `top` boundary, and add values to our result  
- When we reach our `top` boundary we can update our `left` boundary by increasing it by `1`  
- Next, we go `right` and add elements to our result until we reach our `right` boundary  
- Lastly, when the `left` boundary reaches the `right` boundary or `top` reaches `bottom`, we will stop our algorithm because we’ve done every single element

#### Time/ Space complexity
* Time complexity: O(m*n)  
* Space complexity: O(1) extra memory, O(m*n) for the output list  
* Where `m` is the number of rows, and `n` is the number of columns  

#### Thank you for reading! 😊
