+++
title = "LeetCode - 150 - Median of Two Sorted Arrays"
date = 2025-07-11T07:56:09+03:00
tags = ["LeetCode", "150", "Median of Two Sorted Arrays", "Swift"]
draft = false
+++

### The problem

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return [the median](https://en.wikipedia.org/wiki/Median) of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

#### Examples

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

#### Constraints

* nums1.length == m
* nums2.length == n
* 0 <= m <= 1000
* 0 <= n <= 1000
* 1 <= m + n <= 2000
* -10^6 <= nums1\[i], nums2\[i] <= 10^6

#### Explanation

From our given examples we can learn that we could potentially have an even or odd number of total elements after we merge two input arrays.
If we just try to merge both inputs and find the median, it will take us O(m + n) time, but we know that is not what we were asked to do.

In most cases when a problem asks to solve in O(log) time, you need binary search, and that’s exactly the case for this problem.

Let's look closely at the example with input `nums1 = [1, 2, 3, 4, 5], nums2 = [1, 2, 3, 4, 5, 6, 7, 8]`.
We know that we can merge those two arrays and find the median very easily.
The median is the middle value, in our case it will be `4`, because we know that to the left of the middle value we have `six` values, and to the right of it we have `six` values, and the length of the merged array is `13`.
![alt image](images/4.png#center)

In case we had a merged array with length `12` (even) elements, we can take two middle-most values, or in other words the right-most value in our left partition and the left-most value in our right partition, and take the average by adding them together and dividing the sum by `2`.
![alt image](images/4-1.png#center)

### Brute Force Solution

```swift
func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {
    var arr = nums1 + nums2
    arr.sort()

    let n = arr.count
    let mid = n / 2

    if n % 2 == 0 {
        return (Double(arr[mid]) + Double(arr[mid - 1])) / 2
    } else {
        return Double(arr[mid])
    }
}
```

#### Explanation

From the explanation above we learn that the brute force way to solve this problem is by merging two arrays. We will also need to sort the merged array, so the overall time complexity will be O((n + m)\*log(n + m))

#### Time/Space complexity

* Time complexity: O((n + m)\*log(n + m))
* Space complexity: O(n + m)
* Where `n` is the length of `nums1` and `m` is the length of `nums2`

### Binary Search Solution

```swift
class Solution {
    func findMedianSortedArrays(_ nums1: [Int], _ nums2: [Int]) -> Double {
        var A = nums1, B = nums2
        if A.count > B.count {
            swap(&A, &B)
        }

        let total = A.count + B.count
        let half = total / 2
        var l = 0
        var r = A.count

        while true {
            let i = (l + r) / 2
            let j = half - i

            let Aleft = i > 0 ? Double(A[i - 1]) : -Double.infinity
            let Aright = i < A.count ? Double(A[i]) : Double.infinity
            let Bleft = j > 0 ? Double(B[j - 1]) : -Double.infinity
            let Bright = j < B.count ? Double(B[j]) : Double.infinity

            if Aleft <= Bright && Bleft <= Aright {
                if total % 2 == 1 {
                    return min(Aright, Bright)
                } else {
                    return (max(Aleft, Bleft) + min(Aright, Bright)) / 2.0
                }
            } else if Aleft > Bright {
                r = i - 1
            } else {
                l = i + 1
            }
        }
    }
}
```

#### Explanation

We can solve this problem without merging two arrays by using a binary search algorithm, because we know that our given input arrays are in sorted order.
The way we are going to do it is by partitioning our arrays.

* Initially, we are going to find the total number of elements of both arrays and the half value.
* Next, we are going to initialize our `left` and `right` pointers at the start and end of the array accordingly
* Next, we are going to find our `middle` value
* Next, we are going to find our left partition that will contain 3 elements. As for our second array, we do not need to run binary search on that, because if we run binary search on the first array we can compute the size of the left partition of the second array. We can do this by subtracting our calculated `half` value with the length of the current partition.
  ![alt image](images/4-2.png#center)

Now we need to figure out if we found the correct left partition.
The way we are going to do it is by finding out if they are in sorted order.

* We can do this by checking if the right-most value in the second array is greater than or equal to the left-most value of the first array in the right part of the partition.
* We also want to know if the right-most value in the first array's left partition is less than or equal to the left-most value of the second array's right partition.
* In our case the condition is true, and it tells us that our left partition has been done correctly
  ![alt image](images/4-3.png#center)

In case we had the left-most value in the right partition in the second array greater than the left-most value of the right partition in the first array,
we would find the minimum value between those two
![alt image](images/4-4.png#center)

The previous example did not show some edge cases. Let's look at an example with a total of `12` elements.
The length of the first array is `4` therefore the `middle = 2`, and the left portion has a size of `2`

* The size of the left partition in the second array will be `half - size of left partition in the first array (6 - 2 = 4)`
* Now let's check if we have done the left partition correctly. We are going to compare our right-most value with the left-most value, and check if the condition `left-most value <= right-most value` is satisfied
* Next we are going to compare the right-most value with the left-most value and check if the condition `right-most value <= left-most value` is satisfied. In our case it's not true because the condition `4 <= 3` is not true and it tells us that we need to update our pointers
* Next we are going to recompute our `middle` pointer, and our left partitions in both arrays, and check if we partitioned them correctly, by following the same steps as we did before.
  ![alt image](images/4-5.png#center)

To avoid the edge case when the first array is empty, we will be using `Int.max` and `Int.min` values as default values.

#### Time/Space complexity

* Time complexity: O(log(min(n, m)))
* Space complexity: O(1)
* Where `n` is the length of `nums1` and `m` is the length of `nums2`

#### Thank you for reading! 😊
