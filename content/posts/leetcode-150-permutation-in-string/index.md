+++
title = "LeetCode - 150 - Permutation in String"
date = 2025-05-28T07:53:36+03:00
tags = ["LeetCode", "150", "Permutation in String", "Swift"]
draft = false
+++

### The problem  
Given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

> A permutation is a rearrangement of all the characters of a string.

In other words, return `true` if one of `s1`’s permutations is a substring of `s2`.

#### Examples

``` 
Input: s1 = "ab", s2 = "eidbaooo"
Output: true
Explanation: s2 contains one permutation of s1 ("ba").
```

```
Input: s1 = "ab", s2 = "eidboaoo"
Output: false
```

#### Constraints
* 1 <= s1.length, s2.length <= 10^4  
* s1 and s2 consist of lowercase English letters.

#### Explanation  
Before we jump into the solution, let's take a look at the example with input `s1 = "ab", s2 = "eidbaooo"`  
![alt image](images/567.png#center)  
We are looking for a permutation of `s1` in `s2` with the size of `s1`.  
In the example, we can see that we have a permutation of `s1 = "ab"` in `s2 = "eidbaooo"` but in a different order.  
> If we had, for example, characters `"ab"` in `s2`, it would also count as a permutation. Order in this case does not matter.

### Hash Map Solution  
``` swift 
func checkInclusion(_ s1: String, _ s2: String) -> Bool {
    let s2Len = s2.count
    let s2Array = Array(s2)
    var count1: [Character: Int] = [:]

    for c in s1 {
        count1[c, default: 0] += 1
    }

    let need = count1.count

    for i in 0 ..< s2Len {
        var count2 : [Character: Int] = [:]
        var curr = 0

        for j in i ..< s2Len {
            count2[s2Array[j], default: 0] += 1

            if count1[s2Array[j], default: 0] < count2[s2Array[j], default: 0] {
                break
            }

            if count1[s2Array[j], default: 0] == count2[s2Array[j]] {
                curr += 1
            }

            if curr == need {
                return true
            }
        }
    }

    return false
}
```

#### Explanation  
We can solve this problem by using a hashmap.

- We are going to create a hashmap for `s1` which will stay the same.  
- We are also going to have a hashmap for `s2` which will contain only the characters of the current window. So, every time we create a window, we only add the new character and maybe remove the character that was on the left.  
- Once we have those hashmaps, we are going to compare them to check if they are equal.  

This solution will take O(n * m) time complexity, but we can optimize it further and get O(n) time.

#### Time/ Space complexity  
* Time complexity: O(n * m)  
* Space complexity: O(m)  
* Where `n` is the length of string2, and `m` is the length of string1.

### Sliding Window Solution  
``` swift 
func checkInclusion(_ s1: String, _ s2: String) -> Bool {
    if s1.count > s2.count {
        return false
    }

    let s1Array = Array(s1)
    let s2Array = Array(s2)

    let aAsciiValue: Int = Int(Character("a").asciiValue!)

    var count1 = Array(repeating: 0, count: 26)
    var count2 = Array(repeating: 0, count: 26)

    for i in 0 ..< s1.count {
        count1[Int(s1Array[i].asciiValue!) - aAsciiValue] += 1
        count2[Int(s2Array[i].asciiValue!) - aAsciiValue] += 1
    }

    var matches = 0

    for i in 0 ..< 26 {
        if count1[i] == count2[i] {
            matches += 1
        }
    }

    var l = 0
    for r in s1.count ..< s2.count {
        if matches == 26 {
            return true
        }

        var index = Int(s2Array[r].asciiValue!) - aAsciiValue
        count2[index] += 1

        if count1[index] == count2[index] {
            matches += 1
        }

        if count1[index] + 1 == count2[index] {
            matches -= 1
        }

        index = Int(s2Array[l].asciiValue!) - aAsciiValue
        count2[index] -= 1

        if count1[index] == count2[index] {
            matches += 1
        }

        if count1[index] - 1 == count2[index] {
            matches -= 1
        }

        l += 1
    }

    return matches == 26
}
```

#### Explanation  
From the previous solution, we learned that we can get O(n) time with some optimizations.  
- We are going to have the same hashmaps that we had in the previous solution.  
![alt image](images/567-1.png#center)  
- We are also going to have a window of the size of string1, which we will move to the right by one.  

The difference between this solution and the previous one is that we are not going to compare two hashmaps directly. Instead, we are going to keep track of one more variable, `matches`.  
The `matches` variable maintains the total number of equal character counts in both hashmaps.

So, the first thing we do is look at the first few characters and fill up our `s2Count` hashmap.  
![alt image](images/567-2.png#center)  
- After we fill our hashmaps, we iterate through both hashmaps comparing each character.  
- Next, we can see that both hashmaps have matches because they both have **zero** `d`s, `e`s, `f`s, etc.  
- They all match every single character except for `c` and `x`. Therefore, we initially have `24 matches`.

Next, we shift our window one character to the right.  
![alt image](images/567-3.png#center)  
- When we shift, we remove the `b` character from the `s2Count` hashmap.  
- Now, changing the count of `b` in `s2Count` to `0` means it no longer matches the `b` count in the `s1Count` hashmap. Therefore, our `matches` total is updated to `23`.  
- We also added a new character `y`, which affects our **matches** because in `s1Count` we have **zero** `y`s. This means we created a mismatch and need to decrease our `matches` by `1`, making the total number of `matches = 22`.

Next, we continue moving our window and repeat the operations above until we reach `26 matches`, at which point we stop the algorithm and return `true`, or until we reach the end of **string2**, in which case we return `false`.

We also return `false` if **string2** is shorter than **string1**.

#### Time/ Space complexity  
* Time complexity: O(n)  
* Space complexity: O(1)

#### Thank you for reading! 😊
