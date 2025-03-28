+++
title = "LeetCode - Blind 75 - Maximum Product Subarray"
date = 2025-03-28T07:28:21+03:00
tags = ["LeetCode", "Blind 75", "Maximum Product Subarray", "Swift"]
draft = false
+++

### The problem  
Given an integer array `nums`, find a subarray that has the largest product and return the product.  

> A subarray is a contiguous, non-empty sequence of elements within an array.  

The test cases are generated so that the answer will fit in a 32-bit integer.  

#### Examples  

```  
Input: nums = [2,3,-2,4]  
Output: 6  
Explanation: [2,3] has the largest product 6.  
```  

```  
Input: nums = [-2,0,-1]  
Output: 0  
Explanation: The result cannot be 2 because [-2,-1] is not a subarray.  
```  

#### Constraints  
* 1 <= nums.length <= 2 * 10⁴  
* -10 <= nums[i] <= 10  
* The product of any subarray of `nums` is guaranteed to fit in a 32-bit integer.  

### Brute force solution  
```swift  
func maxProduct(_ nums: [Int]) -> Int {  
    var res = nums[0]  
    let n = nums.count  

    for i in 0 ..< n {  
        var curr = nums[i]  
        res = max(res, curr)  
        for j in i + 1 ..< n {  
            curr *= nums[j]  
            res = max(res, curr)  
        }  
    }  

    return res  
}  
```  

#### Explanation  
The brute-force way to solve this problem is to try every single subarray and calculate the product for each. This is not very efficient, and the time complexity will be O(n²).  
![alt image](images/p-152.png#center)  
We can solve this problem in a more optimal way.  

#### Time/space complexity  
* Time complexity: O(n²)  
* Space complexity: O(1)  

### Optimized solution  
```swift  
func maxProduct(_ nums: [Int]) -> Int {  
    var res = nums[0]  
    var currMin = 1  
    var currMax = 1  

    for n in nums {  
        let tmpMax = n * currMax  
        currMax = max(n * currMax, n * currMin, n)  
        currMin = min(tmpMax, n * currMin, n)  
        res = max(res, currMax)  
    }  

    return res  
}  
```  

#### Explanation  
Let’s look at an example where the input is `[1, 2, 3]`.  
![alt image](images/p-152-1.png#center)  
If all numbers are positive, our product will always be increasing. In this case, we can find the result easily by multiplying all of them to get the maximum product.  

Now, let’s consider a scenario where we have all negative numbers: `[-1, -2, -3]`.  
![alt image](images/p-152-2.png#center)  

We can see that:  
- Multiplying the first two elements `-1` and `-2` results in `2`.  
- Multiplying all elements results in `-6`.  
- We also have another subarray that gives a positive number: the last two elements `-2` and `-3`, which result in `6`.  

So, even though we need to find the maximum product subarray, we also need to keep track of the minimum value as well.  

To find the maximum product subarray for the entire input, it helps to solve the subproblem of the first two elements first and then use that result to compute the overall solution.  
![alt image](images/p-152-3.png#center)  

We also need to track the minimum subarray product, which in the case of the first two elements is `-2`. So, we keep track of both the positive and negative values.  

When we move to `-3`, we use the previously calculated values to get a new max (`6`) and a new min (`-6`).  

Even if we had an additional positive `4` in our input `[-1, -2, -3, 4]`, we would take the maximum value at that point (`6`) and multiply it by `4`.  

We must also consider edge cases involving `0`. If we have an input like `[-1, -2, -3, 0, 3, 5]`, we need to reset our product when encountering `0` because multiplying any number by `0` results in `0`. To avoid this issue, we reset the product to `1` when encountering a `0`.  

#### Time/space complexity  
* Time complexity: O(n)  
* Space complexity: O(1)  

#### Thank you for reading! 😊
