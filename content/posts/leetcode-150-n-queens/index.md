+++
title = "LeetCode - 150 - N-Queens"
date = 2025-09-26T08:01:26+03:00
tags = ["LeetCode", "150", "N-Queens", "Swift"]
draft = false
+++

### The problem

The n-queens puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the `n`-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

#### Examples

![alt image](images/queens.jpg#center)

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

```
Input: n = 1
Output: [["Q"]]
```

#### Constraints

* 1 <= n <= 9

#### Explanation

From the description of the problem we learn that we are given `n` different queens and we need to place them in a way that they don’t attack each other.

Before we go solving this problem, let’s refresh our memory of how a queen can move on the board, this will help us come up with base cases and figure out the result of our problem.
![alt image](images/51.png#center)
The queen is allowed to move:

* In any direction horizontally to the left or to the right
* It can move in any direction vertically up or down
* It can also move diagonally

Now when we refreshed our memory about queen moves, we need to place queens in such ways that they don’t attack each other.

* At this point we know that a queen can go horizontally, therefore every queen must be placed in a different row
  ![alt image](images/51-1.png#center)
* We also know that a queen can move vertically, therefore we must place every queen in a separate column
  ![alt image](images/51-2.png#center)
* Lastly, the queen can move diagonally, and this is the hardest part, because each queen must be on a separate diagonal
  ![alt image](images/51-3.png#center)

Now we can try to figure out a way to solve this problem by visualizing an example with `n = 4`

* The brute force way to solve this problem is to try to put a queen at each position in our grid, but it’s very expensive in terms of time complexity because each queen placement will take `O(n^2)`
  ![alt image](images/51-4.png#center)

### Backtracking Solution

```swift
func solveNQueens(_ n: Int) -> [[String]] {
    var col: Set<Int> = []
    var posDiag: Set<Int> = []
    var negDiag: Set<Int> = []
    var res: [[String]] = []
    var board = Array(repeating: Array(repeating: ".", count: n), count: n)

    func backtrack(_ r: Int) {
        if r == n {
            let copy = board.map { $0.joined() }
            res.append(copy)
            return
        }

        for c in 0 ..< n {
            if col.contains(c) || posDiag.contains(r + c) || negDiag.contains(r - c) {
                continue
            }

            col.insert(c)
            posDiag.insert(r + c)
            negDiag.insert(r - c)
            board[r][c] = "Q"

            backtrack(r + 1)

            col.remove(c)
            posDiag.remove(r + c)
            negDiag.remove(r - c)
            board[r][c] = "."
        }
    }

    backtrack(0)
    return res
}
```

#### Explanation

Above we discussed the brute-force way of solving this problem, now we can optimize it to `O(n)` time by using queen placement rules on the board that we recently refreshed.
![alt image](images/51-5.png#center)

* For the first row we can decide to put the queen in any position in this row
* When we move to the second row we can put the queen anywhere in that row except the position that we chose in the first row, because queens will be in the same column.

To do that we will maintain a hashset with `columns` where we have already placed our queens.

We will also keep track of hashsets with `positive diagonal` and `negative diagonal`.
Let’s look at how we can calculate the `negative diagonal`
![alt image](images/51-6.png#center)

If we start from the `top left` position and then we go diagonally `bottom right`, we start from position `0, 0` and then move to position `1, 1`, then to position `2, 2`. You can see that each time we increase the `column` number we also increase the `row` number, therefore we can put it into the formula `row - column`. When we go diagonally we increase `column` by `1` and increase the `row` by `1`.

We can prove that the formula `row - column` is going to work by using different examples:
![alt image](images/51-7.png#center)

* For the diagonal that starts from position `0, 0` every value will be `0`, because `0 - 0 = 0`, `1 - 1 = 0` etc.
* For the diagonal that starts from position `1, 0` every value will be equal to `1`, because `1 - 0 = 1` , `2 - 1 = 1` etc.

You see that we proved that the formula `row - column` works for the `negative diagonal`, now let’s figure out what formula we can use for the `positive diagonal`.

Let’s start our `positive diagonal` from the `bottom left` position with indices `3, 0` and finish it at position `0, 3`
![alt image](images/51-8.png#center)

* You can see that when we are trying to move on the `positive diagonal` we increase the `column` by `1` but we `decrease` the `row` by `1`, therefore our formula `row - column` won’t work, instead we will use the formula `row + column`. We can prove it by looking at examples
  ![alt image](images/51-9.png#center)
* For the `positive diagonal` that starts from position `3, 0` every value will be equal to `3`, because `3 + 0 = 3`, `2 + 1 = 3` etc.
* For the diagonal that starts from `2, 0` every value will be equal to `2`, `2 + 0 = 2`, `1 + 1 = 2`, etc.

Now you can see that we only need `three` hashsets: `columns`, `positive diagonal`, `negative diagonal` to solve this problem.

We will start brute forcing this solution by placing queens using a decision tree
![alt image](images/51-10.png#center)

* At first we are going to try each position in the `first row`, where we have `four` choices `0, 1, 2, 3`
* Next, we will continue doing the same operation, but now we are going to check if a queen has already been placed into the `column set` or `negative diagonal set` or `positive diagonal set`. If a queen has been placed in one of those sets this means that the queens’ paths overlap and we can’t place it at this position.

![alt image](images/51-11.png#center)
For example, if we decided to place the first queen at `0` position in the first row, then we want to place another queen in the second row:

* We know that we have four choices `0, 1, 2, 3`, but as you can see we can’t place the second queen at position `1, 0` because we already have a queen at `column = 0`
* If we try to place the second queen at position `1, 1` we can’t do it because it overlaps with the first queen on the `negative diagonal`
* Lastly, we can place the queen at position `1, 2` or `1, 3` because it does not overlap with the `positive diagonal` or `negative diagonal` and they are not in the same `column`

This algorithm will work for the entire grid.

#### Time/ Space complexity

* Time complexity: O(n!)
* Space complexity: O(n^2)

#### Thank you for reading! 😊
