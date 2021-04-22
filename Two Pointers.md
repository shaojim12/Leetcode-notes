# Two Pointer

* [167. Two Sum II - Input array is sorted (Easy)](https://github.com/shaojim12/Leetcode-notes/blob/master/Two%20Pointers.md#167-two-sum-ii---input-array-is-sorted)
* [633. Sum of Square Numbers (Medium)]()

## 167. Two Sum II - Input array is sorted (Easy)

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1

        while left < right:
            if (numbers[left] + numbers[right]) == target:
                return [left + 1, right + 1]
            elif (numbers[left] + numbers[right]) > target:
                right -= 1
            else:
                left += 1
```

## 633. Sum of Square Numbers (Medium)

The algorithm is same as P.167, but change the value to square.

```python
class Solution:
    def judgeSquareSum(self, c: int) -> bool:
        if c < 0:
            return False
        left, right = 0, int(c**0.5)

        while left <= right:
            if (left**2 + right**2) == c:
                return True
            elif (left**2 + right**2) < c:
                left += 1
            else:
                right -= 1
```
