# Sorting

* [[455] Assign Cookies](https://github.com/shaojim12/Leetcode-notes/blob/master/Greedy.md#455-assign-cookies)
* [[435] Non-overlapping Intervals](https://github.com/shaojim12/Leetcode-notes/blob/master/Greedy.md#435-non-overlapping-intervals)
* [[452] Minimum Number of Arrows to Burst Balloons](https://github.com/shaojim12/Leetcode-notes/blob/master/Greedy.md#452-minimum-number-of-arrows-to-burst-balloons)
* [[406] Queue Reconstruction by Height]()
* [[121] Best Time to Buy and Sell Stock]()
* [[122] Best Time to Buy and Sell Stock II]()
* [[605] Can Place Flowers]()
* [[392] Is Subsequence]()
* [[665] Non-decreasing Array]()
* [[53] Maximum Subarray]()


## **[455] Assign Cookies**

Each child i has a greed factor g[i], which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s[j]. If s[j] >= g[i], we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

```python
Input: g = [1,2,3], s = [1,1]
Output: 1
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```

Pair the cookies to the lowest child first.

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        AssignCookie = 0
        ChildIndex = 0
        CookieIndex = 0
        g = sorted(g)  # sort children
        s = sorted(s)  # sort cookie

        while CookieIndex < len(s) and ChildIndex < len(g):
            # when cookie is greater than child
            if g[ChildIndex] <= s[CookieIndex]:
                AssignCookie += 1
                ChildIndex += 1
                CookieIndex += 1
            else:
                CookieIndex += 1

        return AssignCookie
```

## **[435] Non-overlapping Intervals**

Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

```python
Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
Output: 1
Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
```

First, we sort the intervals by the end time. Then we pick the earliest end time, we can get the maximum non-overlapping intervals.

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        intervals = sorted(intervals, key = lambda x:x[1]) # sort by end time
        removeNum = 0
        current = intervals[0][1]
        
        for i in range(1, len(intervals)):
            if intervals[i][0] < current:
                removeNum += 1
            else:
                current = intervals[i][1]

        return removeNum
```

## **[452] Minimum Number of Arrows to Burst Balloons**

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps traveling up infinitely.

Given an array points where points[i] = [xstart, xend], return the minimum number of arrows that must be shot to burst all balloons.

```python
Input: points = [[10,16],[2,8],[1,6],[7,12]]
Output: 2
Explanation: One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).
```

First, we sort the points by the end time. Then we start from the earliest end time, everytime we pick the earliest end time, we will get the minimum points.

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        if not points:
            return 0
        points = sorted(points, key = lambda x:x[1]) # sort by end 
        current = points[0][1]
        shoots = 1

        for i in range(1, len(points)):
            if current >= points[i][0]:
                continue
            else:
                current = points[i][1]
                shoots += 1

        return shoots
```

## **[406] Queue Reconstruction by Height**

You are given an array of people, people, which are the attributes of some people in a queue (not necessarily in order). Each people[i] = [hi, ki] represents the ith person of height hi with exactly ki other people in front who have a height greater than or equal to hi.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where queue[j] = [hj, kj] is the attributes of the jth person in the queue (queue[0] is the person at the front of the queue).

```python
Input: people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
Output: [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
Explanation:
Person 0 has height 5 with no other people taller or the same height in front.
Person 1 has height 7 with no other people taller or the same height in front.
Person 2 has height 5 with two persons taller or the same height in front, which is person 0 and 1.
Person 3 has height 6 with one person taller or the same height in front, which is person 1.
Person 4 has height 4 with four people taller or the same height in front, which are people 0, 1, 2, and 3.
Person 5 has height 7 with one person taller or the same height in front, which is person 1.
Hence [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] is the reconstructed queue.
```

First, we sort the people descending from the height, and second order is k. Moreover, we insert the people inside the queue by the k order.

```python
class Solution:
    def reconstructQueue(self, people: List[List[int]]) -> List[List[int]]:
        people = sorted(people, key = lambda x:(-x[0], x[1]))

        queue = []
        for peo in people:
            queue.insert(peo[1], peo)

        return queue
```

## **[121] Best Time to Buy and Sell Stock**

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

```python
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        minPrice = prices[0]
        MaxProfit = 0
        
        for i in range(len(prices)):
            MaxProfit = max(MaxProfit, prices[i]-minPrice)
            
            if prices[i] < minPrice:
                minPrice = prices[i]
                
        return MaxProfit
```

## **[122] Best Time to Buy and Sell Stock II**

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

```python
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if not prices:
            return 0
        Profit = 0
        MinPrice = prices[0]
        MaxPrice = 0

        for i in range(1, len(prices)):
            # find the minimum price
            if prices[i] < MinPrice:
                MinPrice = prices[i]
                # when we get the min price, 
                # we have to reset the max price
                MaxPrice = 0 

            # find the maximum price
            elif prices[i] > MaxPrice:
                MaxPrice = prices[i]
            
            # add the profit
            if MaxPrice > MinPrice:
                Profit = Profit + MaxPrice - MinPrice
                MinPrice = prices[i]

        return Profit
```

## **[605] Can Place Flowers**

Given an integer array flowerbed containing 0's and 1's, where 0 means empty and 1 means not empty, and an integer n, return if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule.

```python
Input: flowerbed = [1,0,0,0,1], n = 1
Output: true
```

Smart way to handle the edge.

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        if not flowerbed:
            return False
        # handle for the edge(head and tail)
        flowerbed = [0] + flowerbed + [0] 
        PlantedFlower = 0

        for i in range(1, len(flowerbed) - 1):
            # count the PlantedFlower, if left, self, right are 0
            if flowerbed[i-1] == 0 and flowerbed[i] == 0 and flowerbed[i+1] == 0:
                PlantedFlower += 1
                flowerbed[i] = 1

        return PlantedFlower >= n
```

## **[392] Is Subsequence**

Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).

```python
Input: s = "abc", t = "ahbgdc"
Output: true
```

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        if not s:
            return True

        i = 0
        for letter in t:
            if letter == s[i]:
                i += 1
                if i == len(s):
                    return True

        return False
```

## **[665] Non-decreasing Array**

Given an array nums with n integers, your task is to check if it could become non-decreasing by modifying at most one element.

We define an array is non-decreasing if nums[i] <= nums[i + 1] holds for every i (0-based) such that (0 <= i <= n - 2).

```python
Input: nums = [4,2,3]
Output: true
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
```

```python
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        count = 0 # calculate how much nums should modify

        for i in range(1, len(nums)):
            if nums[i-1] <= nums[i]:
                continue

            count += 1
            # there are two situations, 
            # when self is less than i-2. then modify self become bigger
            # when self is greater than i-2, we can modify i-1 to smaller number.
            if i >= 2 and nums[i-2] > nums[i]:
                nums[i] = nums[i-1]

            else:
                nums[i-1] = nums[i]

        return count <= 1
```

## **[53] Maximum Subarray**

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

```python
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if not nums:
            return 0
        
        MaxSum = nums[0]
        CurrSum = nums[0]

        for i in range(1, len(nums)):
            CurrSum = max(nums[i], CurrSum + nums[i])
            MaxSum = max(MaxSum, CurrSum)

        return MaxSum
```

