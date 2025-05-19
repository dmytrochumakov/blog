+++
title = "LeetCode - 150 - Two Sum II - Input Array Is Sorted"
date = 2025-05-19T07:17:39+03:00
tags = ["LeetCode", "150", "Two Sum II - Input Array Is Sorted", "Swift"]
draft = false
+++

### The problem  
Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

#### Examples

``` 
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

#### Constraints
* 2 <= numbers.length <= 3 * 10^4  
* -1000 <= numbers[i] <= 1000  
* numbers is sorted in non-decreasing order.  
* -1000 <= target <= 1000  
* The tests are generated such that there is exactly one solution.

### Brute Force Solution  
``` swift 
func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
    let n = numbers.count

    for i in 0 ..< n {
        for j in i + 1 ..< n {
            if numbers[i] + numbers[j] == target {
                return [i + 1, j + 1]
            }
        }
    }

    return []
}
```

#### Explanation  
The brute force way to solve this problem is to look through every single combination of two numbers.  
For example, with input `[1, 2, 3, 4, 5, 7, 10, 11]` and `target = 9`:  
![alt image](images/167.png#center)

- We are going to start from the first index with value `1`, and sum it with the next value `2` and check if it's equal to the `target`.  
- We will continue summing values until we reach a `sum` that is more than our `target`, and this also means that we don’t have to look at the remaining numbers in the array, because every next number will be greater than the target.  
- We are going to follow this algorithm with other values until we have tried all possible combinations.

#### Time/Space complexity  
* Time complexity: O(n²)  
* Space complexity: O(1)

### Solution  
``` swift 
func twoSum(_ numbers: [Int], _ target: Int) -> [Int] {
    let n = numbers.count

    var l = 0
    var r = n - 1

    while l < r {
        if numbers[l] + numbers[r] > target {
            r -= 1
        } else if numbers[l] + numbers[r] < target {
            l += 1
        } else {
            return [l + 1, r + 1]
        }
    }

    return []
}
``` 

#### Explanation  
We can solve this problem using the fact that our array is sorted to our advantage.  
Let’s look at our example from above:  
![alt image](images/167-1.png#center)

- First, we eliminated value `11` from consideration.  
- Then we eliminated `10`.  
- So we are basically eliminating elements from the end of the array in reverse order, and we can use this to our advantage.  

Let’s try the same problem but with a slightly different algorithm.  
We can use two pointers where the left pointer is going to be at the beginning of the array, and the right pointer is going to be at the end of the array.  
![alt image](images/167-2.png#center)

Now we are going to sum the values at those pointers:  
- We currently have values `1` and `11`, and `sum = 12`, which is greater than our `target`. Since `sum` is too big, we need to decrease our right pointer, because if we choose to increase our left pointer, we will be increasing `sum`.  
- Next, we recompute our `sum`, which will be `11`, and we will decrease our right pointer again.  
- Next, we will have a sum of `8`, which is less than our target, and then we will increment our left pointer.  
- Lastly, we recompute our `sum` again, where we find our result.

#### Time/Space complexity  
* Time complexity: O(n)  
* Space complexity: O(1)

#### Thank you for reading! 😊
