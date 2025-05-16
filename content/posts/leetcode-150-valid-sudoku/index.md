+++
title = "LeetCode - 150 - Valid Sudoku"
date = 2025-05-16T08:02:42+03:00
tags = ["LeetCode", "150", "Valid Sudoku", "Swift"]
draft = false
+++

### The problem  
Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated according to the following rules:  
1. Each row must contain the digits `1-9` without repetition.  
2. Each column must contain the digits `1-9` without repetition.  
3. Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.  

Note:  
* A Sudoku board (partially filled) could be valid but is not necessarily solvable.  
* Only the filled cells need to be validated according to the mentioned rules.

#### Examples  
![alt image](images/Sudoku-by-L2G-20050714.svg.png#center)  
```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```

```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

#### Constraints  
* board.length == 9  
* board[i].length == 9  
* board[i][j] is a digit 1-9 or '.'.

#### Explanation  
From description of the problem we learn that row must contain digits `1-9` without repetition. The same goes for column, this means that we can't have duplicates in any particular row or column. The last rule tells us that our sub-boxes `3x3` should contain digits `1-9` without repetition.  
Our Sudoku board does not necessarily have to be filled in, and it can have only values without any duplicates.

### Solution  
```swift
func isValidSudoku(_ board: [[Character]]) -> Bool {
    var rows: [Int: Set<Character>] = [:]
    var cols: [Int: Set<Character>] = [:]
    var squares: [IndexHelper: Set<Character>] = [:]

    for r in 0 ..< 9 {
        for c in 0 ..< 9 {
            if board[r][c] == "." {
                continue
            }
            if ( rows[r, default: []].contains(board[r][c])
                 || cols[c, default: []].contains(board[r][c])
                 || squares[IndexHelper(r: r / 3, c: c / 3), default: []].contains(board[r][c])) {
                return false
            }

            cols[c, default: []].insert(board[r][c])
            rows[r, default: []].insert(board[r][c])
            squares[IndexHelper(r: r / 3, c: c / 3), default: []].insert(board[r][c])
        }
    }

    return true
}

struct IndexHelper: Hashable {
    let r: Int
    let c: Int
}
```

#### Explanation  
We can solve this problem by going through every single row and make sure that every particular row does not have any duplicates. We will be using hash set for each single row for entire grid to check if we don't have any duplicates.  
At this point we were able to find a way to follow the first rule and check if row has any duplicates.

- As for second rule where we need to check every single column we can do the same thing, create a hash set for every column, and then we can determine if there are any duplicates or not.

- For the third rule we can also use hash set to store `3x3` grid.

Let's try to figure out how we can use hash set in third rule.  
![alt image](images/36.png#center)

Since we have grid `9x9`, the question becomes how do we know that for example value in `row = 1` and `col = 1` happens to be in particular `3x3` grid, where cell with `row = 4` and `col = 4` happens to be in different grid.

Notice that we have sub-squares with size `3x3`.  
![alt image](images/36-1.png#center)

One way to find value in sub-squares is to have an index that represents row of different squares, and similarly index that represents column.  
Now the question is how we can convert cell with `row = 4` and `col = 4`, to indices.  
Since we have sub-squares with size `3x3`, we are going to take actual index of row `4/3 = 1`, similarly we do the same thing to column `4/3 = 1`, this way we can uniquely identify that `[1, 1]` belongs to the middle `3x3` grid.

Finally, we are going to have hash set where the pair is going to be `row / 3`, `column / 3`, and value is going to be hash set where we can tell do we have any duplicates or not.  
We are going to go through entire grid, if we find any duplicates we return `false`, if we don't we can return `true`.

#### Time/ Space complexity  
* Time complexity: O(n^2)  
* Space complexity: O(n^2)

#### Thank you for reading! 😊
