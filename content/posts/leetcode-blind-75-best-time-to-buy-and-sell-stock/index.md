+++
title = 'LeetCode - Blind 75 - Best Time to Buy and Sell Stock'
date = 2024-11-22T07:14:17+03:00
tags = ["LeetCode", "Blind 75", "Best Time to Buy and Sell Stock", "Swift"]
draft = false
+++

### The problem  
You are given an array `prices` where `prices[i]` is the price of a given stock on the `i-th` day.  
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.  
Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.  

#### Examples  
```  
Input: prices = [7,1,5,3,6,4]  
Output: 5  
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.  
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.  
```  

```  
Input: prices = [7,6,4,3,1]  
Output: 0  
Explanation: In this case, no transactions are done and the max profit = 0.  
```  

#### Constraints  
* 1 <= prices.length <= 10⁵  
* 0 <= prices[i] <= 10⁴  

### Brute force solution  
```swift  
func maxProfit(_ prices: [Int]) -> Int {  
    let n = prices.count  
    var res = 0  

    for i in 0 ..< n {  
        for j in i + 1 ..< n {  
            let profit = prices[j] - prices[i]  
            if profit > 0 {  
                res = max(res, profit)  
            }  
        }  
    }  

    return res  
}  
```  

#### Explanation  
We can start with a brute force solution and find a way to a more optimal solution as we go.  
By visualizing the problem using input from example 1 `[7,1,5,3,6,4]`:  
![alt image](images/problem_121.png#center)  

We can see that the maximum profit is possible if you buy on day `2` for price `1` and sell on day `5` at price `6`, resulting in a profit of `5`.  
We can iterate over all prices, compare the current price with the next one, and calculate the profit. This will work with a time complexity of O(n²), but we can do better by using the two-pointer technique.  

##### Time/Space Complexity  
* **Time complexity:** O(n²)  
* **Space complexity:** O(1)  

### Solution 2 - Two Pointers  
```swift  
func maxProfit(_ prices: [Int]) -> Int {  
    let n = prices.count  
    var res = 0  

    var l = 0  
    var r = 1  

    while r < n {  
        let profit = prices[r] - prices[l]  

        if profit > 0 {  
            res = max(res, profit)  
        } else {  
            l = r  
        }  

        r += 1  
    }  

    return res  
}  
```  

#### Explanation  
From the brute force approach, we learned that it is possible to solve this problem in a more optimal way by using the two-pointer technique. The calculation of profit remains the same, but the solution is optimized to O(n) time complexity.  

##### Time/Space Complexity  
* **Time complexity:** O(n)  
* **Space complexity:** O(1)  

#### Thank you for reading! 😊
