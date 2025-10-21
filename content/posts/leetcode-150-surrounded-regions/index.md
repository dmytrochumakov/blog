+++
title = "LeetCode - 150 - Surrounded Regions"
date = 2025-10-21T07:58:33+03:00
tags = ["LeetCode", "150", "Surrounded Regions", "Swift"]
draft = false
+++

### The problem

You are given an `m x n` matrix `board` containing letters `'X'` and `'O'`, capture regions that are surrounded:

* Connect: A cell is connected to adjacent cells horizontally or vertically.
* Region: To form a region connect every `'O'` cell.
* Surround: The region is surrounded with `'X'` cells if you can connect the region with `'X'` cells and none of the region cells are on the edge of the `board`.

To capture a surrounded region, replace all `'O'`s with `'X'`s in-place within the original board. You do not need to return anything.

#### Examples

```
Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]

Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]

Explanation:
![alt image](images/xogrid.jpg#center)
In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.
```

```
Input: board = [["X"]]

Output: [["X"]]
```

#### Constraints

* m == board.length
* n == board[i].length
* 1 <= m, n <= 200
* board[i][j] is 'X' or 'O'.

#### Explanation

From the description of the problem, we learn that we are given an `m*n` matrix that contains only `X`s and `O`s, and we want to capture all regions that are surrounded by `X`s. Capturing a region is defined by flipping `O`s into `X`s only if `O`s are surrounded by `X`s.
Let’s look at our example:
![alt image](images/130.png#center)

* In this example, we have two different regions. We have two regions because we only mark an area as a region if cells are connected in four directions `(up, down, right, left)`; cells cannot be connected diagonally.

![alt image](images/130-1.png#center)
On the result side of the picture, you can see that the region in the middle was flipped. That region was flipped because it was surrounded by `X`s.

![alt image](images/130-2.png#center)
As for the second region that is placed in the last row, you can see that in the result picture it is not replaced with `X`. We can’t flip that region because it is only connected in three directions — `top, left, right` — but not `bottom`, which is on the border and out of bounds.

### Depth First Search Solution

```swift
func solve(_ board: inout [[Character]]) {
    let rows = board.count
    let cols = board[0].count

    func capture(_ r: Int, _ c: Int) {
        if (r < 0 || c < 0 || r == rows || c == cols || board[r][c] != "O") {
            return
        }
        board[r][c] = "T"
        capture(r - 1, c)
        capture(r + 1, c)
        capture(r , c - 1)
        capture(r , c + 1)
    }

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if (board[r][c] == "O" && ([0, rows - 1].contains(r) || [0, cols - 1].contains(c))) {
                capture(r, c)
            }
        }
    }

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if board[r][c] == "O" {
                board[r][c] = "X"
            }
        }
    }

    for r in 0 ..< rows {
        for c in 0 ..< cols {
            if board[r][c] == "T" {
                board[r][c] = "O"
            }
        }
    }

}
```

#### Explanation

We can start solving this problem in reverse order:

* First, we are going to find every region that we cannot flip.
* Second, we are going to find all regions that we can flip.

We are going to scan through every border cell and look for any `O`s.
![alt image](images/130-3.png#center)
You can see that in our example we have one region that we can’t flip; therefore, we are going to mark it as `T` — a temporary variable that will indicate that this region cannot be flipped.

Next, we are going to iterate through every cell that is not marked as `T` and change it to `X`.
![alt image](images/130-4.png#center)

Lastly, we are going to change back all `T` cells to `O`s, and we will return our result.
![alt image](images/130-5.png#center)

#### Time/Space complexity

* Time complexity: O(m*n)
* Space complexity: O(m*n)
* Where `m` is the number of rows and `n` is the number of columns

#### Thank you for reading! 😊
