# Linked List

* [[160] Intersection of Two Linked Lists]()
* [[19] Remove Nth Node From End of List]()
* [[725] Split Linked List in Parts]()
* [[328] Odd Even Linked List]()

## **[160] Intersection of Two Linked Lists**

Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

```python
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        p1 = headA
        p2 = headB
        # p1 will go through headB and p2 will go through headA, 
        # so the total listNode will be the same.
        # Then, when two pointers bump into each other, 
        # we will find the first intersect node.
        while p1 != p2:
            # when both pointers goes to None, 
            # it means that they don't have inersect node.
            if p1.next == None and p2.next == None:
                return None
            # when p1 is None, we set it to headB, otherwise we go to next.
            if p1.next == None:
                p1 = headB
            else:
                p1 = p1.next
            # when p2 is None, we set it to headA, otherwise we go to next.
            if p2.next == None:
                p2 = headA
            else:
                p2 = p2.next

        return p1
```

## **[19] Remove Nth Node From End of List**

Given the head of a linked list, remove the nth node from the end of the list and return its head.

```python
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        # we should add a dummy node at the front to prevent when we delete the first node.
        dummyHead = ListNode()
        dummyHead.next = head
        p1 = dummyHead
        p2 = dummyHead
        # go through the step between two pointers
        for i in range(n):
            p2 = p2.next
        # when p2 goes to the end, then p1.next will be the correct position
        while p2 != None and p2.next != None:
            p1 = p1.next
            p2 = p2.next
        
        p1.next = p1.next.next # delete a node

        if dummyHead.next == None:
            # when there is no node, we will return None
            return None
        else:
            # otherwise return dummyHead.next
            return dummyHead.next
```

## **[725] Split Linked List in Parts**

Given the head of a singly linked list and an integer k, split the linked list into k consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return an array of the k parts.

```python
Input: head = [1,2,3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but its string representation as a ListNode is [].
```

```python
class Solution:
    def splitListToParts(self, head: Optional[ListNode], k: int) -> List[Optional[ListNode]]:
        if not head:
            return [None for i in range(k)]
        curr_head, length = head, 0

        # get the length
        while curr_head != None:
            curr_head, length = curr_head.next, length + 1

        number, remainder = length // k, length % k # get the numbers of each parts
        ans_list, preNode, remainder_num = [], head, 0 # initialize the parameters

        for i in range(k):
            # add the remainder to the eariler parts
            if i < remainder:
                remainder_num = 1
            else:
                remainder_num = 0

            ans_list.append(head) # add the head into list
            # iterate through the ListNode
            for j in range(number + remainder_num):
                preNode = head
                head = head.next
            
            # Split the Node
            preNode.next = None
                    
        return ans_list
```

## **[328] Odd Even Linked List**

Given the head of a singly linked list, group all the nodes with odd indices together followed by the nodes with even indices, and return the reordered list.

The first node is considered odd, and the second node is even, and so on.

Note that the relative order inside both the even and odd groups should remain as it was in the input.

You must solve the problem in O(1) extra space complexity and O(n) time complexity.

```python
Input: head = [2,1,3,5,6,4,7]
Output: [2,3,6,7,1,5,4]
```

```python
class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head:
            return None

        oddNode = head 
        evenNode, evenHead = head.next, head.next

        while evenNode != None and evenNode.next != None:
            oddNode.next = evenNode.next
            evenNode.next = oddNode.next.next
            oddNode = oddNode.next
            evenNode = evenNode.next

        oddNode.next = evenHead

        return head
```