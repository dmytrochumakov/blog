+++
title = "LeetCode - 150 - Koko Eating Bananas"
date = 2025-07-02T07:51:15+03:00
tags = ["LeetCode", "150", "Koko Eating Bananas", "Swift"]
draft = false
+++

### The problem

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

#### Examples

```
Input: piles = [3,6,7,11], h = 8  
Output: 4
```

```
Input: piles = [30,11,23,4,20], h = 5  
Output: 30
```

```
Input: piles = [30,11,23,4,20], h = 6  
Output: 23
```

#### Constraints

* 1 <= piles.length <= 10^4
* piles.length <= h <= 10^9
* 1 <= piles\[i] <= 10^9

#### Explanation

From the description of the problem we know that we have `n` piles of bananas given to us in the input array `piles[i]`. 

We are also given the second parameter `h`, which is the number of hours that we have in order to eat all of the bananas.

Koko has an eating speed at which she can eat a certain amount of bananas per hour, and that variable is `k`, where `k` is the value that we are trying to determine as the solution to this problem.

We also learn that Koko can only eat `one` entire pile of bananas in one hour.

### Brute Force Solution

```swift
func minEatingSpeed(_ piles: [Int], _ h: Int) -> Int {
    var speed = 1

    while true {
        var totalTime = 0

        for pile in piles {
            totalTime += Int(ceil(Double(pile) / Double(speed)))
        }

        if totalTime <= h {
            return speed
        }

        speed += 1
    }

    return speed
}
```

#### Explanation

Let's look at the first example and try to figure out the way we can solve this problem.
![alt image](images/875.png#center)

The brute force way is to start from `k = 1`. 

This way Koko can only eat `3 / 1 = 3`, plus `6 / 1 = 6`, that will be `9` and it's more than `h = 8`, so this way Koko can only eat `one` pile.

We know that the minimum `k` value can only be `1` because that would mean that Koko is barely eating any piles. 

As for the maximum value for `k`, it will be the maximum value in our input array.

To solve this problem, we are going to try to iterate over `k` in the range `1 … max(piles)`, and we will continue until we find our value that allows us to eat every single pile in less than or equal to our `h` value. 

The time complexity for this solution will be O(m\*n).

But why should we iterate over every single element in the `k` range when we have our target `h`, and we can apply binary search algorithm and optimize our solution?

#### Time/ Space complexity

* Time complexity:  O(m\*n)
* Space complexity:  O(1)
* where `m` is the length of the input array and `n` is the maximum number of bananas in a pile.

### Binary Search Solution

```swift
func minEatingSpeed(_ piles: [Int], _ h: Int) -> Int {
    var l = 1
    var r = piles.max()!
    var res = r

    while l <= r {
        let m = (r + l) / 2
        var hours = 0

        for p in piles {
            hours += Int(ceil(Double(p) / Double(m)))
        }

        if hours <= h {
            res = min(res, m)
            r = m - 1
        } else {
            l = m + 1
        }
    }

    return res
}
```

#### Explanation

From the solution above we learn that we can apply the binary search algorithm and reduce time complexity to `O(logm * n)`.

We know that the potential rate `k` that Koko eats bananas is going to be in the range from `1` to `max(piles)` (in our example it's `11`).

* Next, we are going to have the `left` pointer at the start and the `right` pointer at the end of the range.
* Next, we compute the middle pointer, and our `k`. So for middle value `6`, our rate of eating bananas will be `6` and our `res` value will be `6`. We are not going to stop because we are looking for the minimum `k` value.
  ![alt image](images/875-1.png#center)
* Now, we can discard all elements on the right side, and only look at the left side. Our middle pointer will be at value `3`, and the rate of eating bananas will be `3/3 + 6/3 + 7/3 + 11/3 = 10`. The rate is greater than our `h`, so we are going to update our `left` and middle pointers.
  ![alt image](images/875-2.png#center)
* Next, we repeat the steps we did before and calculate the rate that will result in value `8`. So at this point our middle pointer is at `4`, and our result is `6`, so we can update it to `4`.
  ![alt image](images/875-3.png#center)
* Lastly, we are going to stop our binary search and return the result because if we continue, we will be searching values that we already crossed out.

#### Time/ Space complexity

* Time complexity: O(log(m \* n))
* Space complexity:  O(1)

#### Thank you for reading! 😊
