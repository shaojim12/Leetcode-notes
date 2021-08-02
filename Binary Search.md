# Binary Search

* [[69] Sqrt(x)](https://github.com/shaojim12/Leetcode-notes/blob/master/Binary%20Search.md#69-sqrtx)
* [[744] Find Smallest Letter Greater Than Target](https://github.com/shaojim12/Leetcode-notes/blob/master/Binary%20Search.md#744-find-smallest-letter-greater-than-target)
* [[540] Single Element in a Sorted Array](https://github.com/shaojim12/Leetcode-notes/blob/master/Binary%20Search.md#540-single-element-in-a-sorted-array)
* [[278] First Bad Version](https://github.com/shaojim12/Leetcode-notes/blob/master/Binary%20Search.md#278-first-bad-version)
* [[34] Find First and Last Position of Element in Sorted Array](https://github.com/shaojim12/Leetcode-notes/blob/master/Binary%20Search.md#34-find-first-and-last-position-of-element-in-sorted-array)


Binary search can find a target in O(log(n)).  
The purpose is cut the list into half each time. And shift the right or left pointr to middle.

## **[69] Sqrt(x)**

Given a non-negative integer x, compute and return the square root of x.

Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.

Note: You are not allowed to use any built-in exponent function or operator, such as pow(x, 0.5) or x ** 0.5.

```python
Input: x = 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
```


```python
class Solution:
    def mySqrt(self, x: int) -> int:
        if x == 0:
            return 0
        left, right = 0, x
        
        while True:
            mid = left + (right - left) // 2
            if (mid*mid) <= x < ((mid+1)*(mid+1)):
                return mid

            elif x < (mid*mid):
                right = mid - 1
            else:
                left = mid + 1
```

## **[744] Find Smallest Letter Greater Than Target**

Given a characters array letters that is sorted in non-decreasing order and a character target, return the smallest character in the array that is larger than target.

```python
Input: letters = ["c","f","j"], target = "a"
Output: "c"
Input: letters = ["c","f","j"], target = "c"
Output: "f"
Input: letters = ["c","f","j"], target = "j"
Output: "c"
```

```python
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        left, right = 0, len(letters)-1

        while left <= right:
            mid = left + (right - left) // 2
            if letters[mid] > target:
                right = mid - 1
            else:
                left = mid + 1

        if left <= len(letters)-1:
            return letters[left]
        else:
            return letters[0]
```

## **[540] Single Element in a Sorted Array**

You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.

```python
Input: nums = [1,1,2,3,3,4,4,8,8]
Output: 2
Input: nums = [3,3,7,7,10,11,11]
Output: 10
```

There are four cases,
* when mid is equal to mid - 1, and find whether left or right is odd number left
* when mid is equal to mid + 1, and find whether left or right is odd numbers left

```python
class Solution:
    def singleNonDuplicate(self, nums: List[int]) -> int:
        left, right = 0, len(nums)-1

        while left < right:
            mid = left + (right-left) // 2

            if nums[mid] == nums[mid-1]:
                if (mid % 2) == 0:
                    right = mid - 2
                else:
                    left = mid + 1

            elif nums[mid] == nums[mid+1]:
                if (mid % 2) == 0:
                    left = mid + 2
                else:
                    right = mid - 1
            
            else:
                return nums[mid]

        return nums[left]
```

## **[278] First Bad Version**

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

```python
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

```python
class Solution:
    def firstBadVersion(self, n):
        if n == 0:
            return 0
        left, right = 1, n
        while left < right:
            mid = left + (right - left) // 2
            if isBadVersion(mid) == True and isBadVersion(mid-1) == False:
                return mid
            elif isBadVersion(mid) == True:
                right = mid - 1
            else:
                left = mid + 1

        return left
```

## **[34] Find First and Last Position of Element in Sorted Array**

Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

```python
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums:
            return [-1, -1]
        left, right = 0, len(nums)-1
        
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                break
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1

        left = right = mid
        if nums[mid] != target:
            return [-1, -1]
        else:
            while left > 0 and nums[left-1] == target:
                left -= 1
            while  right < (len(nums)-1) and nums[right+1] == target:
                right += 1
            return [left, right]
```

