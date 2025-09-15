+++
title = "LeetCode - 150 - Letter Combinations of a Phone Number"
date = 2025-09-15T07:36:50+03:00
tags = ["LeetCode", "150", "Letter Combinations of a Phone Number", "Swift"]
draft = false
+++

### The problem

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![alt image](images/1200px-telephone-keypad2svg.png#center)

#### Examples

```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

```
Input: digits = ""
Output: []
```

```
Input: digits = "2"
Output: ["a","b","c"]
```

#### Constraints

* 0 <= digits.length <= 4
* digits\[i] is a digit in the range \['2', '9']

#### Explanation

From the description of the problem, we learn that we are given a string with digits from `2-9` and we need to return all possible combinations that the number could represent.

Let's look at the example with `digits = "23"`
![alt image](images/17.png#center)
For the single digit `2` we can have `three` different combinations, the same goes for digit `3`, in total we can have `3 * 3 = 9` combinations.

### Backtracking Solution

```swift
func letterCombinations(_ digits: String) -> [String] {
    var res: [String] = []
    var digitToChar: [Character: String] = [
        "2": "abc",
        "3": "def",
        "4": "ghi",
        "5": "jkl",
        "6": "mno",
        "7": "pqrs",
        "8": "tuv",
        "9": "wxyz"
    ]
    let n = digits.count

    func backtrack(_ i: Int, _ currStr: String) {
        if currStr.count == n {
            res.append(currStr)
            return
        }

        for c in digitToChar[digits[i]]! {
            backtrack(i + 1, currStr + String(c))
        }

    }

    if !digits.isEmpty {
        backtrack(0, "")
    }

    return res
}
extension StringProtocol {
    subscript(offset: Int) -> Character { self[index(startIndex, offsetBy: offset)] }
}
```

#### Explanation

We can solve this problem by using the backtracking algorithm.

The first step that we need to take care of is to create a hashmap that will map every digit in the range `2-9` to its characters.

Now let's look at the decision tree for the same example as above `"23"`
![alt image](images/17-1.png#center)

* The first character `2` can map to `three` different characters `a, b, c` therefore we can have `three` different choices.
* At the second position, we have character `3` that also has `three` different characters `d, e, f`, so now every character from the first position `a, b, c` can be mapped to every character from the second position `d, e, f`.

As a result, every leaf node in our decision tree will be added into our output.

#### Time/ Space complexity

* Time complexity: O(n\*4^n)
* Space complexity: O(n) extra space, O(n\*4^n) space for the output array

#### Thank you for reading! 😊
