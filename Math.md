# Math

* [[198] House Robber]()




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