# Dynamic Programming

* [[198] House Robber]()
* [[213] House Robber II]()
* [[303] Range Sum Query - Immutable]()
* [[413] Arithmetic Slices]()
* [[343] Integer Break]()
* [[376] Wiggle Subsequence]()
* [[1143] Longest Common Subsequence]()
* [[416] Partition Equal Subset Sum]()
* [[494] Target Sum]()
* [[474] Ones and Zeroes]()
* [[518] Coin Change 2]()
* [[377] Combination Sum IV]()
* [[309] Best Time to Buy and Sell Stock with Cooldown]()
* [[714] Best Time to Buy and Sell Stock with Transaction Fee]()
* [[123] Best Time to Buy and Sell Stock III]()
* [[188] Best Time to Buy and Sell Stock IV]()
* [[583] Delete Operation for Two Strings]()
* [[72] Edit Distance]()

## **[198] House Robber**

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

```python
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        # preprocessing for the dp
        if not nums:
            return 0
        elif len(nums) == 1:
            return nums[0]
        elif len(nums) == 2:
            return max(nums[0], nums[1])
        if nums[0] > nums[1]:
            nums[1] = nums[0]
        # OPT(i) = max(OPT(i-2) + nums[i], OPT(i-1))
        for i in range(2, len(nums)):
            nums[i] = max(nums[i-2] + nums[i], nums[i-1])

        return nums[-1]
```

## **[213] House Robber II**

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

```python
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        if not nums:
            return 0
        elif len(nums) <= 3:
            return max(nums)

        round1 = [nums[0]]
        round2 = [nums[1]]
        if nums[0] > nums[1]:
            round1.append(nums[0])
        else:
            round1.append(nums[1])

        if nums[1] > nums[2]:
            round2.append(nums[1])
        else:
            round2.append(nums[2])
        
        # OPT(i) = max(OPT(i-2) + nums[i], OPT(i-1))
        for i in range(2, len(nums)-1):
            round1.append(max(round1[i-2] + nums[i], round1[i-1]))
        
        for i in range(2, len(nums)-1):
            round2.append(max(round2[i-2] + nums[i+1], round2[i-1]))

        return max(round1[-1], round2[-1])
```

## **[303] Range Sum Query - Immutable**

Given an integer array nums, handle multiple queries of the following type:

Calculate the sum of the elements of nums between indices left and right inclusive where left <= right.
Implement the NumArray class:

NumArray(int[] nums) Initializes the object with the integer array nums.
int sumRange(int left, int right) Returns the sum of the elements of nums between indices left and right inclusive (i.e. nums[left] + nums[left + 1] + ... + nums[right]).

```python
Input
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
Output
[null, 1, -1, -3]

Explanation
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3
```

```python
class NumArray:
    def __init__(self, nums: List[int]):
        # for the first element, we put 0, 
        # then all the numbers will push 1 step right
        self.accu = [0]  
        for num in nums:
            self.accu.append(self.accu[-1] + num) 

    def sumRange(self, left: int, right: int) -> int:
        # the total of 0~right minus 0~left
        return self.accu[right+1] - self.accu[left]
```

## **[413] Arithmetic Slices**

An integer array is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, [1,3,5,7,9], [7,7,7,7], and [3,-1,-5,-9] are arithmetic sequences.
Given an integer array nums, return the number of arithmetic subarrays of nums.

A subarray is a contiguous subsequence of the array.

```python
Input: nums = [1,2,3,4]
Output: 3
Explanation: We have 3 arithmetic slices in nums: [1, 2, 3], [2, 3, 4] and [1,2,3,4] itself.
```

```python
class Solution:
    def numberOfArithmeticSlices(self, nums: List[int]) -> int:
        result = 0
        opt = [0, 0]
        # use 1D array to store the opt of every element
        # when there is four element that is arithmetic, 
        # then it will add two to result.
        # if five continuous element that is arithmetic, 
        # then it will add three to result.
        for i in range(2, len(nums)):
            if nums[i]-nums[i-1] == nums[i-1]-nums[i-2]:
                opt.append(opt[-1]+1)
                result += opt[-1]
            else:
                opt.append(0)

        return result
```

## **[343] Integer Break**

Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.

Return the maximum product you can get.

```python
Input: n = 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```

```python
class Solution:
    def integerBreak(self, n: int) -> int:
        # create a 1D array to store each maximum product
        dp = [0 for i in range(n+1)]
        
        for i in range(2, n+1): # iterate through every numbers
            for j in range(1, i): # iterate through 1~(i-1)
                # compare three things every time, 
                # max(self, two elements, many elements)
                dp[i] = max(dp[i], j * (i-j), j * dp[i-j])

        return dp[-1]
```

## **[300] Longest Increasing Subsequence**

Given an integer array nums, return the length of the longest strictly increasing subsequence.

A subsequence is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, [3,6,2,7] is a subsequence of the array [0,3,1,6,2,2,7].

```python
Input: nums = [10,9,2,5,3,7,101,18]
Output: 4
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # create a 1D array and init with 1
        dp = [1 for i in range(len(nums))]

        # iterate through every number, 
        # in every number, we iterate through 0~self, 
        # then we find the maximum value with lower number, 
        # and add one to it.
        for i in range(len(nums)):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j]+1)

        return max(dp)
```

## **[376] Wiggle Subsequence**

A wiggle sequence is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.

For example, [1, 7, 4, 9, 2, 5] is a wiggle sequence because the differences (6, -3, 5, -7, 3) alternate between positive and negative.
In contrast, [1, 4, 7, 2, 5] and [1, 7, 4, 5, 5] are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.
A subsequence is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.

Given an integer array nums, return the length of the longest wiggle subsequence of nums.

```python
Input: nums = [1,17,5,10,13,15,10,5,16,8]
Output: 7
Explanation: There are several subsequences that achieve this length.
One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).
```

```python
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        if not nums:
            return 0
        up = [1 for i in range(len(nums))] # store the number that goes up
        down = [1 for i in range(len(nums))] # store the number that goes down

        for i in range(1, len(nums)):
            # when the number goes up, we find the biggest down and add one to it
            if nums[i] > nums[i-1]:
                up[i] = down[i-1] + 1
                down[i] = down[i-1]
            # when the number goes down, we find the biggest up and add one to it
            elif nums[i] < nums[i-1]:
                down[i] = up[i-1] + 1
                up[i] = up[i-1]
            # when the number is same as the previous number, we don't change anything
            else:
                down[i] = down[i-1]
                up[i] = up[i-1]

        return max(up[-1], down[-1])
```

## **[1143] Longest Common Subsequence**

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

```python
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # create a 2d array with i+1 row and j+1 column
        # first row and column set as 0
        dp = [[0 for i in range(len(text2) + 1)] for j in range(len(text1) + 1)]

        # when the character is same, we add one to dp[i-1][j-1], 
        # when the character is not same, we pick the bigger number from dp[i-1][j] and dp[i][j-1]
        for i in range(1, len(text1) + 1):
            for j in range(1, len(text2) + 1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])

        return dp[-1][-1]
```

## **[416] Partition Equal Subset Sum**

Given a non-empty array nums containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

```python
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

```python
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)
        # we exclude the odd number
        if total % 2 == 1:
            return False
        target = total // 2
        # create a 2D array, 
        # the row will be nums, 
        # the column will be 0 to target
        dp = [[False for i in range(target+1)] for j in range(len(nums)+1)] 

        # preprocess the array, 
        # when the number is zero, all the situation will be True
        for i in range(len(nums)+1):
            dp[i][0] = True

        # if the up cell is True, the current cell can be True, 
        # and when the column number minus the nums of that row is True, the current cell can also be True
        for i in range(1, len(nums)+1):
            for j in range(1, target+1):
                if dp[i-1][j] == True:
                    dp[i][j] = True
                elif j - nums[i-1] >= 0 and dp[i-1][j-nums[i-1]] == True:
                    dp[i][j] = True
            # when the last column become true in any row, we can return True immediately
            if dp[i][-1] == True:
                return True

        return False
```

## **[494] Target Sum**

You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
Return the number of different expressions that you can build, which evaluates to target.

```python
Input: nums = [1,1,1,1,1], target = 3
Output: 5
Explanation: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

```python
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        total = sum(nums)
        if (total + target) % 2 == 1:
            return 0
        elif target > total:
            return 0

        dp = [[0 for i in range(2*total+1)] for j in range(len(nums)+1)] # create 2D array
        dp[0][total] = 1 # init the middle as 1
        
        # every row means each nums
        # every column means the possible sum it will have
        # we start from the first row, and add, minus the first number to second row, 
        # then, we add and minus each non-zero number at second row to third row.
        # repeat it until last row, and return the target number
        for i in range(len(nums)):
            for j in range(2*total+1):
                if dp[i][j] != 0:
                    dp[i+1][j-nums[i]] += dp[i][j]
                    dp[i+1][j+nums[i]] += dp[i][j]

        return dp[-1][total+target]
```

## **[474] Ones and Zeroes**

You are given an array of binary strings strs and two integers m and n.

Return the size of the largest subset of strs such that there are at most m 0's and n 1's in the subset.

A set x is a subset of a set y if all elements of x are also elements of y.

```python
Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.
```

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        if not strs:
            return 0
        # create 2D array, 
        # the row is how many zeros, 
        # the column is how many ones
        dp = [[0 for i in range(n+1)] for j in range(m+1)] 

        # from each grid, we find the current grid minus it's ones and zeros', we will get another grid 
        # and add one to that number, then the number will be the possible answer of that grid
        # finally the number that we find out will compare to the original number and choose the bigger one
        for str in strs:
            zeros, ones = self.countStrs(str)
            for i in range(m, zeros-1, -1):
                for j in range(n, ones-1, -1):
                    dp[i][j] = max(dp[i-zeros][j-ones] + 1, dp[i][j])
        
        return dp[-1][-1]        

    def countStrs(self, str):
        # a helper that use to count how many zeros and ones
        zeros = 0
        ones = 0
        
        for i in range(len(str)):
            if str[i] == "0":
                zeros += 1
            else:
                ones += 1
        return zeros, ones
```

## **[518] Coin Change 2**

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

```python
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

```python
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        dp = [[0 for i in range(amount+1)] for j in range(len(coins)+1)] # create 2D array
        dp[0][0] = 1 # preprocess the first grid as zero

        # we start from first coins and add all the possible situation on it, 
        # such as 2, 4, 6, 8
        # then we iterate through second coin, then from every grid, we minus the sencod coin and find the number,
        # then we add [i-1][j] = [i][j-(sencond coin)]
        # continue to finish the whole array
        for i in range(1, len(dp)):
            for j in range(len(dp[0])):
                if j - coins[i-1] >= 0:
                    dp[i][j] = dp[i-1][j] + dp[i][j-coins[i-1]]
                else:
                    dp[i][j] = dp[i-1][j]

        return dp[-1][-1]
```

## **[377] Combination Sum IV**

Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target.

The answer is guaranteed to fit in a 32-bit integer.

```python
Input: nums = [1,2,3], target = 4
Output: 7
Explanation:
The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
Note that different sequences are counted as different combinations.
```

```python
# first solution with 2D array
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        column = target // min(nums) # get the total column, it will be the target divide minimum number
        if column == 0:
            return 0
        dp = [[0 for i in range(target+1)] for j in range(column)]

        for num in nums:
            if num <= target:
                dp[0][num] = 1

        for i in range(column-1):
            for j in range(target+1):
                if dp[i][j] != 0:
                    for num in nums:
                        if j+num <= target:
                            dp[i+1][j+num] += dp[i][j]

        ans = 0 
        for solution in dp:
            ans += solution[-1]

        return ans
```

```python
# better solution with 1D array, it is simple and easy, the runinng time is also O(target*len(nums)), space is O(target)
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [0 for i in range(target+1)] # create a 1D array
        dp[0] = 1 # preprocess the first row to one
        # then iterate through all the grid, and add all different number in nums to it
        # we then will get all the possible solutions.
        # Another thought, we will get all the possible result on each number from zero to target, 
        # for example, when we iterate through 0, then it will have (1, 2, 3),
        # when we iterate through 1, it will be (1+1, 1+2, 1+3, 2, 3), 
        # when we iterate through 2, it will be (1+1+1, 1+1+2, 2+1, 2+2, 1+2, 1+3, 3), 
        # when we iterate through 3, it will be (1+1+1+1, 1+1+2, 2+1+1, 2+2, 1+2+1, 1+3, 3+1)
        # we had delete all the sum that is bigger than the target
        for i in range(target):
            for num in nums:
                if i+num <= target:
                    dp[i+num] += dp[i]

        return dp[target]
```

## **[309] Best Time to Buy and Sell Stock with Cooldown**

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

 

```python
Input: prices = [1,2,3,0,2]
Output: 3
Explanation: transactions = [buy, sell, cooldown, buy, sell]
```

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        sold = float('-inf')
        held = float('-inf')
        reset = 0
        # sold status comes from held.
        # held status comes from held or buy.
        # reset status comes from sold or keep reset.
        for price in prices:
            presold = sold
            sold = held + price
            held = max(held, reset-price)
            reset = max(presold, reset)

        return max(sold, held, reset)
```

## **[714] Best Time to Buy and Sell Stock with Transaction Fee**

You are given an array prices where prices[i] is the price of a given stock on the ith day, and an integer fee representing a transaction fee.

Find the maximum profit you can achieve. You may complete as many transactions as you like, but you need to pay the transaction fee for each transaction.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

```python
Input: prices = [1,3,2,8,4,9], fee = 2
Output: 8
Explanation: The maximum profit can be achieved by:
- Buying at prices[0] = 1
- Selling at prices[3] = 8
- Buying at prices[4] = 4
- Selling at prices[5] = 9
The total profit is ((8 - 1) - 2) + ((9 - 4) - 2) = 8.
```

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        # initialize the process
        held = float('-inf')
        wait = 0
        sold = float('-inf')
        # After we buy stock, we can held or sold stock. 
        # After we wait, we can keep waiting or we can buy the stock.
        # After we sold the stock, we can wait not to buy, or we can buy stock immediately.

        # If the held[i] status is comming from wait[i-1] or sold[i-1], it have to minus the fee, 
        # why max(wait, sold) is because when we but the stock, we have to compare their price, 
        # for example, if there is a lower price after price[i+1], it means wait[i] will be better.
        # But if selling the stock do not make more money, we will choose to hold, it will be held[i-1].

        # The wait[i] status is comming from max(wait[i-1], sold[i-1]), 
        # this is because, if we make money by sold, we sold it, 
        # but sometimes we won't make any money when we sold it at sold[i-1], it means we would rather wait for next turn.
        
        # The sold[i] status can only come from held[i-1]. 
        # We will sold the stock every turn, but it will be useful only when we earn the money.   
        for price in prices:
            preheld = held
            held = max((max(wait, sold) - price - fee), held)
            wait = max(wait, sold)
            sold = preheld + price

        return max(held, wait, sold)
```

## **[123] Best Time to Buy and Sell Stock III**

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete at most two transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

```python
Input: prices = [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
```

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        length = len(prices) - 1
        left = [0 for i in range(len(prices))]
        right = [0 for i in range(len(prices))]

        left_min = prices[0]
        right_max = prices[-1]
        # we find all the maximum price from left to right, and right from left
        # then we add all the left[i] + right[i+1] together to find the maximum profit.
        for i in range(1, len(prices)):
            left[i] = max(left[i-1], prices[i] - left_min)
            left_min = min(left_min, prices[i])

            right[length - i] = max(right[length - i + 1], right_max - prices[length - i])
            right_max = max(right_max, prices[length - i])

        ans = 0
        for i in range(len(prices)-1):
            ans = max(ans, left[i] + right[i+1])

        return max(ans, left[-1], right[0])
```

## **[188] Best Time to Buy and Sell Stock IV**

You are given an integer array prices where prices[i] is the price of a given stock on the ith day, and an integer k.

Find the maximum profit you can achieve. You may complete at most k transactions.

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

```python
Input: k = 2, prices = [3,2,6,5,0,3]
Output: 7
Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
```

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if not k or not prices:
            return 0
        # create a 3D array, dp[day_number][used_transaction][stock_holding]
        # stock_holding: 0 = not holding, 1 = holding
        dp = [[[float('-inf') for k in range(2)] for j in range(k+1)] for i in range(len(prices))]

        dp[0][0][0] = 0 # when no transaction at first day
        dp[0][1][1] = - prices[0] # when we buy stock at first day

        # every [day_number][used_transaction][stock_holding] is the best result in that day
        # There are two situations when we do not hold the stock, 
        # 1. keep not holding the stock,             dp[i][j][0] = dp[i-1][j][0]
        # 2. sell the stock from the holding state.  dp[i][j][0] = dp[i-1][j][1] + prices[i]
        # Then we have to find the bigger one, dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i])
        
        # Also there are two situations when we already hold a stock.
        # 1. keep holding the stock,                   dp[i][j][1] = dp[i-1][j][1]
        # 2. buy the stock from the not holding stock. dp[i][j][1] = dp[i-1][j-1][0] - prices[i]
        # Then we have to find the bigger one, dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i])
        # The first time we bought the stock, we will use a transaction, so we skip j = 0, we start from j = 1
        for i in range(1, len(prices)):
            for j in range(k+1):
                dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i])

                if j > 0:
                    dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i])

        return max(dp[-1][j][0] for j in range(k+1))
```

## **[583] Delete Operation for Two Strings**

Given two strings word1 and word2, return the minimum number of steps required to make word1 and word2 the same.

In one step, you can delete exactly one character in either string.

```python
Input: word1 = "sea", word2 = "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
```

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        dp = [[0 for i in range(len(word1) + 1)] for j in range(len(word2) + 1)]
        
        # find the equal characters,
        # and use length(word1) + length(word2) minus the maximum equal character * 2
        # why times 2 to maximum equal characters
        for i in range(1, len(word2) + 1):
            for j in range(1, len(word1) + 1):
                if word2[i-1] == word1[j-1]:
                    dp[i][j] = max(dp[i-1][j-1]+1, dp[i][j-1], dp[i-1][j])
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])

        return len(word1) + len(word2) - dp[-1][-1]*2
```

## **[72] Edit Distance**

Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.

You have the following three operations permitted on a word:

Insert a character
Delete a character
Replace a character

```python
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

```python
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        # create 2D array fo dynamic programming
        dp = [[0 for i in range(len(word1) + 1)] for j in range(len(word2) + 1)]

        # preprocess the array
        for i in range(len(dp)):
            dp[i][0] = i
        
        for j in range(len(dp[0])):
            dp[0][j] = j

        # in each cell, we put the correct answer on it, 
        # we start from the first row, 
        # if word2 is None, then every word should delete, so it will be 0, 1, 2, 3, ...
        # Then if there is one row, and the character is same, 
        # we will find the minimum from dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1]. 
        # dp[i-1][j]+1, dp[i][j-1]+1 is because we don't need this same character, 
        # dp[i-1][j-1] means we will use that same character, so we get the [i-1][j-1] position

        # when both character is different, 
        # we if we get the value from dp[i-1][j], dp[i][j-1], it means we delete the current one, 
        # if we get from dp[i-1][j-1], means we replace it
        for i in range(1, len(dp)):
            for j in range(1, len(dp[0])):
                if word1[j-1] == word2[i-1]:
                    dp[i][j] = min(dp[i-1][j]+1, dp[i][j-1]+1, dp[i-1][j-1])
                else:
                    dp[i][j] = min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) + 1

        return dp[-1][-1]

```

## **[72] Edit Distance**

There is only one character 'A' on the screen of a notepad. You can perform two operations on this notepad for each step:

Copy All: You can copy all the characters present on the screen (a partial copy is not allowed).
Paste: You can paste the characters which are copied last time.
Given an integer n, return the minimum number of operations to get the character 'A' exactly n times on the screen.

```python
Input: n = 3
Output: 3
Explanation: Intitally, we have one character 'A'.
In step 1, we use Copy All operation.
In step 2, we use Paste operation to get 'AA'.
In step 3, we use Paste operation to get 'AAA'.

```

```python
class Solution:
    def minSteps(self, n: int) -> int:
        if n == 1:
            return 0
        half = n//2
        # create 2D array to do dynamic programming
        # column means A, A, A, A, A, A, A, ......
        # row means A, AA, AAA, AAAA, ......
        dp = [[0 for i in range(n)] for j in range(half)]

        # preprocess the first row as 0, 2, 3, 4, 5, 6, ......
        for i in range(1, n):
            dp[0][i] = i+1

        # we iterate through each unit in every row, 
        # we start from second row.
        for i in range(1, half):
            for j in range(1, n):
                if ((j+1) % (i+1)) == 0:
                    # if the column number can divide by row number, that means it is its multiple number,
                    # so we find the mimimum from dp[i][i] + ((j+1) // (i+1)) (multiple number) and the top one dp[i-1][j]
                    if (j+1) // (i+1) != 1:
                        dp[i][j] = min(dp[i][i] + ((j+1) // (i+1)), dp[i-1][j])
                    else:
                        # if the column and row is directly the same, it means we want the minimum number from it's top value dp[i-1][j]
                        dp[i][j] = dp[i-1][j]
                else:
                    # if the column number is not the multiple number from row, we just get the minimum number from it's top value dp[i-1][j]
                    dp[i][j] = dp[i-1][j]

        return dp[-1][-1]
```