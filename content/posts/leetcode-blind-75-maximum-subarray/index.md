+++
title = "LeetCode - Blind 75 - Maximum Subarray"
date = 2025-04-10T07:40:25+03:00
tags = ["LeetCode", "Blind 75", "Maximum Subarray", "Swift"]
draft = false
+++

### The problem  
Given an integer array `nums`, find the subarray with the largest sum, and return its sum.

> A subarray is a contiguous non-empty sequence of elements within an array.

#### Examples

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]  
Output: 6  
Explanation: The subarray [4,-1,2,1] has the largest sum 6.
```

```
Input: nums = [1]  
Output: 1  
Explanation: The subarray [1] has the largest sum 1.
```

```
Input: nums = [5,4,-1,7,8]  
Output: 23  
Explanation: The subarray [5,4,-1,7,8] has the largest sum 23.
```

#### Constraints  
* 1 <= nums.length <= 10⁵  
* -10⁴ <= nums[i] <= 10⁴  

Follow up: If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

#### Explanation  
We can solve this problem in a brute force way by computing every single subarray for every single value. That algorithm will take O(n³) time complexity.
For example, with input `nums = [-2,1,-3,4,-1,2,1,-5,4]`, visual representation will look like this:  
![alt image](images/53.png#center)

### Brute Force (Optimized) Solution  
``` swift 
func maxSubArray(_ nums: [Int]) -> Int {
    let n = nums.count
    var res = nums[0]

    for i in 0 ..< n {
        var curr = 0
        for j in i ..< n {
            curr += nums[j]
            res = max(res, curr)
        }
    }

    return res
}
```

#### Explanation  
We can slightly optimize the brute force solution with O(n³) time.

Let’s look at how we can do it:

- We are going to start from index `0` and iterate over the entire array,  
- Inside this loop we will have another loop and a property that will keep track of the current sum `curr`,  
- And we will update it after each iteration of the loop.

This solution will take O(n²) time complexity, but we can do better.

Now the question that we need to ask ourselves is: do we need to start at every single value and compute every single subarray, or can we avoid repetitive work?

#### Time/ Space complexity  
* Time complexity: O(n²)  
* Space complexity: O(1)

### Kadane’s Algorithm Solution  
``` swift 
func maxSubArray(_ nums: [Int]) -> Int {
    var maxSum = nums[0]
    var currSum = 0

    for n in nums {
        if currSum < 0 {
            currSum = 0
        }
        currSum += n
        maxSum = max(maxSum, currSum)
    }

    return maxSum
}
```

#### Explanation  
In the previous solution, we learned that we can optimize the brute force solution with O(n²) to linear time of O(n) by eliminating repetitive work.  
Let’s look at the example with input `nums = [-2,1,-3,4,-1,2,1,-5,4]`  
![alt image](images/53-1.png#center)

- We start at a value with the negative number `-2` and if we sum it with the next value of `1`, we will receive a negative value of `-1`. So we can conclude that we can basically ignore the negative prefix value because it is not going to help us find the result, so we are not going to consider it.  

- Next, we move to the value of `1` and try to find the max subarray with the value of `-3`, but we can see that when we sum them we receive a negative `-2` value, so this means that we can skip those values and move to the next.  

- Now, we move to the value of `4` and sum it with the negative value of `-1`, that will result in `3`. After that, we continue without skipping because our sum resulted in a positive value.  
- Next, we use our current sum and add the positive value of `2` to it, so the result will be `5`.  
- Next, we add `1` to our current sum of `5`, that results in `6`.  
- Next, we add negative `-5` to our sum, that results in positive `1`.  
- Lastly, we add `4` to our sum, and now our result equals `5`.

This algorithm takes linear time, where we go through the array once, removing the negative prefix as we compute the total sum.

#### Time/ Space complexity  
* Time complexity: O(n)  
* Space complexity: O(1)

#### Thank you for reading! 😊
