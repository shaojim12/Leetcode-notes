# Math

* [[204] Count Primes]()
* [[504] Base 7]()
* [[405] Convert a Number to Hexadecimal]()
* [[168] Excel Sheet Column Title]()
* [[172] Factorial Trailing Zeroes]()
* [[462] Minimum Moves to Equal Array Elements II]()
* [[169] Majority Element]()
* [[367] Valid Perfect Square]()
* [[326] Power of Three]()
* [[628] Maximum Product of Three Numbers]()

## **[204] Count Primes**

Count the number of prime numbers less than a non-negative number, n.

```python
Input: n = 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```

```python
import math
class Solution:
    def countPrimes(self, n: int) -> int:
        if n < 3:
            return 0
        
        number = {}
        for p in range(2, int(math.sqrt(n)) + 1):
            if p not in number:
                for multiple in range(p*p, n, p):
                    number[multiple] = 1

        return n - len(number) - 2
```

## **[504] Base 7**

Given an integer num, return a string of its base 7 representation.

```python
Input: num = 100
Output: "202"
Input: num = -7
Output: "-10"
```

```python
class Solution:
    def convertToBase7(self, num: int) -> str:
        if num < 0:
            restNum = -1 * num
        else:
            restNum = num

        remainder = 0
        base7 = []
        while restNum // 7 > 0:
            remainder = restNum % 7
            restNum = restNum // 7
            base7.insert(0, str(remainder)) # insert the number in the front

        base7.insert(0, str(restNum))
        # if want to be string, we have to add string into the array
        # otherwise, we can "".join(str(i) for i in base7)
        if num < 0:
            return "-" + "".join(base7) 
        else:
            return "".join(base7)
```

## **[405] Convert a Number to Hexadecimal**

Given an integer num, return a string representing its hexadecimal representation. For negative integers, two’s complement method is used.

All the letters in the answer string should be lowercase characters, and there should not be any leading zeros in the answer except for the zero itself.

Note: You are not allowed to use any built-in library method to directly solve this problem.

```python
Input: num = 26
Output: "1a"

Input: num = -1
Output: "ffffffff"
```

```python
class Solution:
    def toHex(self, num: int) -> str:
        d = {0:'0', 1:'1', 2:'2', 3:'3', 4:'4', 5:'5', 6:'6', 7:'7', 8:'8', 9:'9', 
        10:'a', 11:'b', 12:'c', 13:'d', 14:'e', 15:'f'}

        if num == 0:
            return "0"
        # the step to calculate the complement is 
        # when the number is negative, 
        # we will change all the 0 to 1, 1 to 0, and add one to that number, 
        # than it will be the nagative number
        if num < 0:
            num = 2**32 + num

        result = []
        while num != 0:
            result.insert(0, d[num%16])
            num = num // 16

        return "".join(result)
```

## **[168] Excel Sheet Column Title**

Given an integer columnNumber, return its corresponding column title as it appears in an Excel sheet.

```python
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

```python
class Solution:
    def convertToTitle(self, columnNumber: int) -> str:
        p = {1:'A', 2:'B', 3:'C', 4:'D', 5:'E', 6:'F', 7:'G', 8:'H', 9:'I', 10:'J', 
        11:'K', 12:'L', 13:'M', 14:'N', 15:'O', 16:'P', 17:'Q', 18:'R', 19:'S', 20:'T', 
        21:'U', 22:'V', 23:'W', 24:'X', 25:'Y', 0:'Z'}

        result = []
        while columnNumber > 0:
            
            result.insert(0, p[columnNumber%26])
            if columnNumber % 26 == 0:
                columnNumber = columnNumber // 26 - 1
            else:
                columnNumber = columnNumber // 26

        return "".join(result)
```

## **[172] Factorial Trailing Zeroes**

Given an integer n, return the number of trailing zeroes in n!.

Follow up: Could you write a solution that works in logarithmic time complexity?

```python
Input: n = 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```

```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        result = 0

        while n > 0:
            result += n // 5
            n = n // 5

        return result
```

## **[462] Minimum Moves to Equal Array Elements II**

Given an integer array nums of size n, return the minimum number of moves required to make all array elements equal.

In one move, you can increment or decrement an element of the array by 1.

Test cases are designed so that the answer will fit in a 32-bit integer.

```python
Input: nums = [1,2,3]
Output: 2
Explanation:
Only two moves are needed (remember each move increments or decrements one element):
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

```python
class Solution:
    def minMoves2(self, nums: List[int]) -> int:
        nums.sort()

        mid = len(nums) // 2
        ans = 0
        # find the midium number, and add all the number between them
        for i in range(len(nums)):
            if i < mid:
                ans += (nums[mid] - nums[i])
            elif i > mid:
                ans += (nums[i] - nums[mid])

        return ans
```

## **[169] Majority Element**

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

```python
Input: nums = [2,2,1,1,1,2,2]
Output: 2
```

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        nums.sort()
        return nums[len(nums) // 2]
```

## **[367] Valid Perfect Square**

Given a positive integer num, write a function which returns True if num is a perfect square else False.

Follow up: Do not use any built-in library function such as sqrt.

```python
Input: num = 16
Output: true
```

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        square = 1

        while square*square <= num:
            if square*square == num:
                return True
            elif square*square < num:
                square += 1

        return False
```

## **[326] Power of Three**

Given an integer n, return true if it is a power of three. Otherwise, return false.

An integer n is a power of three, if there exists an integer x such that n == 3x.

```python
Input: n = 27
Output: true
```

```python
class Solution:
    def isPowerOfThree(self, n: int) -> bool:
        while n > 1:
            n = n / 3

        return True if n == 1 else False
```

## **[628] Maximum Product of Three Numbers**

Given an integer array nums, find three numbers whose product is maximum and return the maximum product.

```python
Input: nums = [1,2,3]
Output: 6
```

```python
class Solution:
    def maximumProduct(self, nums: List[int]) -> int:
        nums.sort()

        return max(nums[-1] * nums[-2] * nums[-3], nums[-1] * nums[0] * nums[1])
```