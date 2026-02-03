+++
title = "LeetCode - 150 - Best Time to Buy and Sell Stock with Cooldown"
date = 2026-02-03T11:42:54+03:00
tags = ["LeetCode", "150", "Best Time to Buy and Sell Stock with Cooldown", "2D_Dynamic_Programming", "Swift"]
draft = false
+++

### The problem

You are given an array `prices` where `prices[i]` is the price of a given stock on the `i`-th day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

* After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

```
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

**Example 2:**

```
Input: prices = [1]
Output: 0
```

**Constraints:**

* `1 <= prices.length <= 5000`
* `0 <= prices[i] <= 1000`

#### Explanation

From the description of the problem, we learn that we are given an array of `prices` and we need to maximize profit. We also learn that we can make as many transactions as we like, but with a couple of restrictions: **we can't buy a stock and sell it on the same day**, we must have **one cooldown day**, and **we can't buy multiple stocks simultaneously**; we must buy and sell stocks one by one.

### Dynamic Programming (Top-Down) Solution

```swift
func maxProfit(_ prices: [Int]) -> Int {
    var dp: [Helper: Int] = [:]

    func dfs(_ i: Int, _ buying: Bool) -> Int {
        if i >= prices.count {
            return 0
        }

        if let cached = dp[Helper(i: i, buying: buying)] {
            return cached
        }

        let cooldown = dfs(i + 1, buying)
        if buying {
            let buy = dfs(i + 1, false) - prices[i]
            dp[Helper(i: i, buying: buying)] = max(buy, cooldown)
        } else {
            let sell = dfs(i + 2, true) + prices[i]
            dp[Helper(i: i, buying: buying)] = max(sell, cooldown)
        }

        return dp[Helper(i: i, buying: buying)]!
    }

    return dfs(0, true)
}

struct Helper: Hashable {
    let i: Int
    let buying: Bool
}
```

#### Explanation

We have three choices: we can **buy** or **sell** stock, or we can have a **cooldown** and do nothing.

We can better understand the problem by visualizing it in the form of a decision tree.
![alt image](images/309.png#center)

* If we decide to buy stock on the first day (that has price `1`), our **profit equals -1**.
* If we decide not to buy stock on the first day (by using **cooldown**), our **profit equals 0**.
  ![alt image](images/309-1.png#center)
* On the second day, we can choose to **sell**, **buy** stock, or use **cooldown**.
  ![alt image](images/309-2.png#center)
* If we sell stock on the second day, we must do a day of **cooldown**, and only after that can we **buy** a new stock.
  ![alt image](images/309-3.png#center)
* Lastly, if we continue moving along our previous path, we can **sell** or take a day of **cooldown**.

While we are moving recursively along our decision tree, we are going to find the **maximum price** in leaf nodes and update our result.

In order to **buy** stock, we are going to increment our index by `1`; as for **selling**, we are going to increment the index by `2`.

#### Time/ Space complexity

* Time complexity: O(n)
* Space complexity: O(n)

#### Thank you for reading! 😊
