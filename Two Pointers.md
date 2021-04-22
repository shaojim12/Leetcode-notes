# Two Pointers

* [167. Two Sum II - Input array is sorted](https://github.com/shaojim12/Leetcode-notes/blob/master/Two%20Pointers.md#167-two-sum-ii---input-array-is-sorted)

## 167. Two Sum II - Input array is sorted

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
