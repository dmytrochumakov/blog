+++
title = "LeetCode - Blind 75 - Word Search"
date = 2025-02-07T10:25:00+03:00
tags = ["LeetCode", "Blind 75", "Word Search", "Swift"]
draft = false
+++

### The Problem  
Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.  

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.  

#### Examples  

![alt image](images/word2.jpg#center)  
```  
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"  
Output: true  
```  

![alt image](images/word-1.jpg#center)  
```  
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"  
Output: true  
```  

![alt image](images/word3.jpg#center)  
```  
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"  
Output: false  
```  

#### Constraints  
- `m == board.length`  
- `n == board[i].length`  
- `1 <= m, n <= 6`  
- `1 <= word.length <= 15`  
- `board` and `word` consist of only lowercase and uppercase English letters.  

**Follow-up:** Could you use search pruning to make your solution faster with a larger board?  

### Backtracking Hash Set Solution  
```swift  
func exist(_ board: [[Character]], _ word: String) -> Bool {  
    let ROWS = board.count  
    let COLS = board[0].count  
    let word = Array(word)  
    let wordCount = word.count  
    var path: Set<[Int]> = []  

    func dfs(_ row: Int, _ col: Int, _ i: Int) -> Bool {  
        if i == wordCount {  
            return true  
        }  

        if (row < 0 || col < 0 || row >= ROWS || col >= COLS  
            || word[i] != board[row][col] || path.contains([row, col])) {  
            return false  
        }  

        path.insert([row, col])  

        let res = (dfs(row + 1, col, i + 1) ||  
                   dfs(row - 1, col, i + 1) ||  
                   dfs(row, col + 1, i + 1) ||  
                   dfs(row, col - 1, i + 1))  

        path.remove([row, col])  

        return res  
    }  

    for r in 0..<ROWS {  
        for c in 0..<COLS {  
            if dfs(r, c, 0) {  
                return true  
            }  
        }  
    }  

    return false  
}  
```  

#### Explanation  
When a problem mentions a grid, matrix, or graph, it usually means that we need a **backtracking algorithm** because we can explore four different directions (top, left, right, down) to find neighboring cells.  

We start from `0,0` (row and column) and try to move in different directions if possible. To do that, we use **depth-first search (DFS)** with a few base cases:  
1. If `i` equals `wordCount`, we found the word.  
2. If the algorithm goes out of bounds of the board, return `false`.  
3. We insert our path, move recursively in four directions, and return the result.  

##### Time / Space Complexity  
- **Time Complexity:** O(m * 4^n)  
- **Space Complexity:** O(n)  
- Where `m` is the number of cells in the board and `n` is the length of the word.  

---

### Backtracking Optimized Solution  
```swift  
func exist(_ board: [[Character]], _ word: String) -> Bool {  
    let ROWS = board.count  
    let COLS = board[0].count  
    var board = board  
    let wordArray = Array(word)  
    let wordArrayCount = wordArray.count  

    func dfs(_ r: Int, _ c: Int, _ i: Int) -> Bool {  
        if i == wordArrayCount {  
            return true  
        }  
        if r < 0 || c < 0 || r >= ROWS || c >= COLS || board[r][c] != wordArray[i] || board[r][c] == "#" {  
            return false  
        }  

        let temp = board[r][c]  
        board[r][c] = "#"  

        let res = dfs(r + 1, c, i + 1) ||  
                  dfs(r - 1, c, i + 1) ||  
                  dfs(r, c + 1, i + 1) ||  
                  dfs(r, c - 1, i + 1)  

        board[r][c] = temp  
        return res  
    }  

    for r in 0..<ROWS {  
        for c in 0..<COLS {  
            if dfs(r, c, 0) {  
                return true  
            }  
        }  
    }  

    return false  
}  
```  

#### Explanation  
We can optimize memory in the initial solution by removing the **hash set** and using **input modification** instead. We mark the value of the current cell with a `#` symbol so that we know it has been visited. The rest of the algorithm stays the same.  

##### Time / Space Complexity  
- **Time Complexity:** O(m * 4^n)  
- **Space Complexity:** O(n)  
- Where `m` is the number of cells in the board and `n` is the length of the word.  

#### Thank you for reading! 😊
