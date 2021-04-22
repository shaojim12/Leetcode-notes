# Two Pointer

* [167. Two Sum II - Input array is sorted (Easy)](https://github.com/shaojim12/Leetcode-notes/blob/master/Two%20Pointer.md#167-two-sum-ii---input-array-is-sorted-easy)
* [633. Sum of Square Numbers (Medium)](https://github.com/shaojim12/Leetcode-notes/blob/master/Two%20Pointer.md#633-sum-of-square-numbers-medium)
* [345. Reverse Vowels of a String (Easy)]()
* [680. Valid Palindrome II (Easy)]()

## 167. Two Sum II - Input array is sorted (Easy)

Problem: Given an array of integers numbers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.

When the number is bigger than target, right pointer minus one.
Otherwise left pointer plus one.

```python
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
```

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

Problem: Given a non-negative integer c, decide whether there're two integers a and b such that a2 + b2 = c.

The algorithm is same as P.167, but change the value to square.

```python
Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
```

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

## 345. Reverse Vowels of a String (Easy)

Problem: Given a string s, reverse only all the vowels in the string and return it.

String is not available to change their characters, so we use s = list(s) to change its type from string to list. Then at the bottom of the algorithm, we use join to combine list to string.

### **join()** function

```python
list1 = ['1','2','3','4'],
print("-".join) 
output: 1-2-3-4

list1 = ['1','2','3','4']
print("".join)
output: 1234
```

```python
Input: s = "hello"
Output: "holle"
```

```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        if not s:
            return s
        
        s = list(s)
        left, right = 0, len(s)-1
        m = "aeiouAEIOU"

        while left < right:
            if s[left] in m and s[right] in m:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1

            elif s[left] not in m:
                left += 1

            elif s[right] not in m:
                right -= 1

        return "".join(s)
```

## 680. Valid Palindrome II (Easy)

Problem: Given a string s, you may delete at most one character. Whether you can make it a palindrome.

We have to make a helper to iterate through two situations, one is left+1, other one is right-1.  
Time: O(n), Space: O(1)

```python
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        left, right = 0, len(s)-1

        while left < right:
            if s[left] == s[right]:
                left += 1
                right -= 1
            else:
                return self.validPalindromeUtil(s, left + 1, right) or self.validPalindromeUtil(s, left, right-1)
        
        return True

    def validPalindromeUtil(self, s, left, right):
        while left < right:
            if s[left] == s[right]:
                left += 1
                right -= 1
            else:
                return False

        return True
```
