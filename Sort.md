# Sorting

* [215. Kth Largest Element in an Array (Medium)]()
* [347. Top K Frequent Elements (Medium)]()
* [451. Sort Characters By Frequency (Medium)]()
* [75. Sort Colors]()



## **215. Kth Largest Element in an Array (Medium)**

Given an integer array nums and an integer k, return the kth largest element in the array.

```python
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5
```

Use quick sort to sort the numbers, and skip some part that is outside our purpose k to save running time.

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        self.quick_sort(0, len(nums)-1, nums, k)
        return nums[k-1]


    def partition(self, start, end, nums):
        pivot = nums[end]  # pick the last number be the pivot
        pivot_index = start - 1  # calculate how many number is bigger than pivot
        for j in range(start, end):
            if pivot <= nums[j]:
                pivot_index += 1
                nums[pivot_index], nums[j] = nums[j], nums[pivot_index]
                
        pivot_index += 1
        nums[end], nums[pivot_index] = nums[pivot_index], nums[end]
        return pivot_index
    

    def quick_sort(self, start, end, nums, k):
        if start < end:
            p = self.partition(start, end, nums) # get the position of the last number

            # this can save some time, 
            # we don't need to sort the number that are outside k
            if p == (k-1):
                return 
            elif p > (k-1):
                self.quick_sort(start, p-1, nums, k)
            else:
                self.quick_sort(p+1, end, nums, k)
```

## **347. Top K Frequent Elements (Medium)**

Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.

```python
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```

First save each numbers into a dictionary, and sort the value of the dictionary.

```python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        dic = {}
        for tmp in nums:
            if tmp in dic:
                dic[tmp] += 1
            else:
                dic[tmp] = 1

        dic = sorted(dic, key=dic.get)
        
        answer = []
        for i in range(k):
            answer.append(dic[len(dic)-1-i])

        return answer
```

## **451. Sort Characters By Frequency (Medium)**

Given a string s, sort it in decreasing order based on the frequency of characters, and return the sorted string.

```python
Input: s = "tree"
Output: "eert"
Explanation: 'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```

First count the frequency of the letter by hash map, then use lambda to sort the hash map.  

dict(sorted(cha_dict.items(), key = lambda x:x[?])

* when ? is 0, sort by key.
* when ? is 1, sort by value.

Last we use an array and join() function to build the string.

```python
class Solution:
    def frequencySort(self, s: str) -> str:
        if not s:
            return s
        
        # count the frequency of the letter
        cha_dict = {}
        for cha in s:
            if cha not in cha_dict:
                cha_dict[cha] = 1
            else:
                cha_dict[cha] += 1

        # sort the hash map by value
        cha_dict = dict(sorted(cha_dict.items(), key = lambda x:x[1], reverse=True))
        # use array to build the string
        StringBuilder = []
        for letter in cha_dict:
            StringBuilder.append(letter * cha_dict[letter])
        
        return "".join(StringBuilder)
```

## **75. Sort Colors**

Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.

```python
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        p1 = 0  # to store the zero's position
        curr = 0  # current position
        p2 = len(nums) - 1  # to store the two's position

        # the current position will start at 0, 
        # and iterate through all the numbers until it is over p2's position, 
        # when curr is over p2, it means all the zeros and ones are found.
        while curr <= p2:
            # when the currnet number is zero, we swap curr and p1
            if nums[curr] == 0:
                nums[p1], nums[curr] = nums[curr], nums[p1]
                p1 += 1
                curr += 1
            # when the current number is two, we swap curr and p2
            elif nums[curr] == 2:
                nums[p2], nums[curr] = nums[curr], nums[p2]
                p2 -= 1
            else:
                curr += 1
        
        return nums
```
