+++
title = "LeetCode - 150 - Design Twitter"
date = 2025-08-25T07:54:06+03:00
tags = ["LeetCode", "150", "Design Twitter", "Swift"]
draft = false
+++

### The problem

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and are able to see the `10` most recent tweets in the user's news feed.

Implement the `Twitter` class:

* `Twitter()` Initializes your Twitter object.
* `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
* `List<Integer> getNewsFeed(int userId)` Retrieves the `10` most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themselves. Tweets must be ordered from most recent to least recent.
* `void follow(int followerId, int followeeId)` The user with ID `followerId` starts following the user with ID `followeeId`.
* `void unfollow(int followerId, int followeeId)` The user with ID `followerId` stops following the user with ID `followeeId`.

#### Examples

```
Input
["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
[[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]
Output
[null, null, [5], null, null, [6, 5], null, [5]]

Explanation
Twitter twitter = new Twitter();
twitter.postTweet(1, 5); // User 1 posts a new tweet (id = 5).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5]. return [5]
twitter.follow(1, 2);    // User 1 follows user 2.
twitter.postTweet(2, 6); // User 2 posts a new tweet (id = 6).
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 2 tweet ids -> [6, 5]. Tweet id 6 should precede tweet id 5 because it is posted after tweet id 5.
twitter.unfollow(1, 2);  // User 1 unfollows user 2.
twitter.getNewsFeed(1);  // User 1's news feed should return a list with 1 tweet id -> [5], since user 1 is no longer following user 2.
```

#### Constraints

* 1 <= userId, followerId, followeeId <= 500
* 0 <= tweetId <= 10^4
* All the tweets have unique IDs.
* At most 3 \* 10^4 calls will be made to postTweet, getNewsFeed, follow, and unfollow.
* A user cannot follow himself.

#### Explanation

Let's start with the easy part of this problem that are `follow` and `unfollow` operations and move up as we go.

We can start implementing `follow` and `unfollow` operations by using a hashmap data structure where we will be using `userId` as the key.
![alt image](images/355.png#center)

We also know that a user can follow multiple users, therefore we can use an array to store them but it will not be very efficient, because deleting from the array will take `O(n)` time. Instead of using an array we can use a hashset that will help us optimize deletion time from `O(n)` to `O(1)`.
![alt image](images/355-1.png#center)

At this stage we figure out the implementation of `follow` and `unfollow` operations, now let's move to the `postTweet` operation.

Each user can post a tweet, so we need to be able to map a user to their array of tweets.

In this case an array is working for us because we do not need to search or delete tweets, when we add a new tweet it only takes `O(1)` time because we add it to the end of the array.
![alt image](images/355-2.png#center)

An array of tweets will also be useful when we will be trying to get the last `10` tweets in the `getNewsFeed` operation because the most recent tweets will be at the end of the array.

### Min Heap Solution

```swift
class Twitter {

    private var count: Int = 0
    private var tweetMap: [Int: [(Int, Int)]] = [:]
    private var followMap: [Int: Set<Int>] = [:]

    init() {}

    func postTweet(_ userId: Int, _ tweetId: Int) {
        tweetMap[userId, default: []].append((count, tweetId))
        count += 1
    }

    func getNewsFeed(_ userId: Int) -> [Int] {
        var res: [Int] = []
        var minHeap: Heap<Helper> = []

        followMap[userId, default: []].insert(userId)

        for followeeId in followMap[userId]! {
            if let tweets = tweetMap[followeeId], !tweets.isEmpty {
                let index = tweets.count - 1
                let (count, tweetId) = tweets[index]
                minHeap.insert(
                    Helper(
                        count: count,
                        tweetId: tweetId,
                        followeeId: followeeId,
                        index: index - 1
                    )
                )
            }
        }

        while !minHeap.isEmpty && res.count < 10 {
            let helper = minHeap.popMin()!
            res.append(helper.tweetId)

            if helper.index >= 0 {
                let (nextCount, nextTweetId) = tweetMap[helper.followeeId]![helper.index]
                minHeap.insert(
                    Helper(
                        count: nextCount,
                        tweetId: nextTweetId,
                        followeeId: helper.followeeId,
                        index: helper.index - 1
                    )
                )
            }
        }

        return res
    }

    func follow(_ followerId: Int, _ followeeId: Int) {
        followMap[followerId, default: []].insert(followeeId)
    }

    func unfollow(_ followerId: Int, _ followeeId: Int) {
        followMap[followerId]?.remove(followeeId)
    }

}

struct Helper: Comparable {
    static func < (lhs: Helper, rhs: Helper) -> Bool {
        return lhs.count > rhs.count 
    }
    
    let count: Int
    let tweetId: Int
    let followeeId: Int
    let index: Int
}
```

#### Explanation

From the explanation above we learn what data structures we will be using for `follow`, and `unfollow` operations now let's explore how we can implement `getNewsFeed` and `postTweet` operations in more detail.

When we are trying to `getNewsFeed` with `10` most recent tweets from users that the current `userId` it's possible that we can have multiple arrays with `tweetId`s.
![alt image](images/355-3.png#center)

The problem with using an array for the `tweetId`s is that we can't only compare `tweetId` because we don't know when the tweet was posted. Instead, we are going to store a pair of values - the time when that tweet was posted, and `tweetId`. We can find time by calculating the total number of `tweetId`s.
![alt image](images/355-4.png#center)

To be able to find an array of tweets we need to find who the user with the given `userId` is following - we can do this by using our hashmap with a set of `followeeId`s. After that, we can iterate over `followeeId`s and find the list of tweets from our second hashmap.
![alt image](images/355-5.png#center)

In case we had `6` recent tweets or fewer we will be returning that number.
In case we had more than `10` tweets we will be returning the `10` most recent ones.

We can solve this problem by using a min heap data structure.
Let's look at an example when we have arrays with tweets and we need to find the most recent ones.
![alt image](images/355-6.png#center)

To find the most recent tweets we are going to start iterating from the end of the array because the most recent tweets always will be at the end.

* Next, we will add the most recent values to the min heap
  ![alt image](images/355-7.png#center)
* Lastly, we are going to find a `minimum` value and add it to our result

#### Time/ Space complexity

* Time complexity: O(n) for each `getNewsFeed` operation, and O(1) for remaining operations
* Space complexity: O(N*m + N*M + n)
* Where `n` is the total number of `followeeId`s associated with `userId`, `m` is the maximum number of tweets by any user (`m` can be at most `10`), `N` is the total number of `userId`s, and `M` is the maximum number of followees for any user.


#### Thank you for reading! 😊
