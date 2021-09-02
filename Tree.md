# Tree

* [[110] Balanced Binary Tree]()
* [[543] Diameter of Binary Tree]()
* [[226] Invert Binary Tree]()
* [[617] Merge Two Binary Trees]()
* [[112] Path Sum]()
* [[437] Path Sum III]()


## **[110] Balanced Binary Tree**

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

```python
Input: root = [3,9,20,null,null,15,7]
Output: true
```

```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        # create a helper function to find all the length of the root
        def isBalancedHelper(root, length):  
            # when the length is -1, it means we don't need to calculate anymore
            if length == -1:
                return -1
            # when the root is None, we return the real length
            if not root:
                return length - 1

            leftLength = isBalancedHelper(root.left, length + 1)
            rightLength = isBalancedHelper(root.right, length + 1)
            # when both root get to the None, we will need to check if there length differ is no more than 1
            # if it's below one, we can continue the process, 
            # if not, it means we don't need to calcuate anymore, we return -1
            if abs(leftLength - rightLength) > 1:
                return -1
            else:
                return max(leftLength, rightLength)

        if isBalancedHelper(root, 1) == -1:
            return False
        else:
            return True
```

## **[543] Diameter of Binary Tree**

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

```python
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

```python
class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        self.maxLength = 0

        # create a helper function to find longest path
        def diameterOfBinaryTreeHelper(root, length):
            if not root:
                return length - 1

            leftLength = diameterOfBinaryTreeHelper(root.left, length + 1)
            rightLength = diameterOfBinaryTreeHelper(root.right, length + 1)
            # at each node, we check if they have the longest path that go through this root.
            # length*2 means the length from the current root to the topest root, so we will need to minus it
            self.maxLength = max(self.maxLength, leftLength + rightLength - length*2)
            return max(leftLength, rightLength)

        diameterOfBinaryTreeHelper(root, 0)

        return self.maxLength
```

## **[226] Invert Binary Tree**

Given the root of a binary tree, invert the tree, and return its root.

```python
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        # just switch left root and right root
        def invertTreeHelper(root):
            if root == None:
                return
            if root.left == None and root.right == None:
                return
            root.right, root.left = root.left, root.right

            invertTreeHelper(root.left)
            invertTreeHelper(root.right)

        invertTreeHelper(root)

        return root
```

## **[617] Merge Two Binary Trees**

You are given two binary trees root1 and root2.

Imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not. You need to merge the two trees into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of the new tree.

Return the merged tree.

Note: The merging process must start from the root nodes of both trees.

```python
Input: root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
Output: [3,4,5,5,4,null,7]
```

```python
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if root1 == None:
            return root2
        # Only three situations will happen,
        # 1. if root1 and root2 exist, we sum the value.
        # 2. if only root1 exist, we return it.
        # 3. if only root2 exist, we let the root1 be root2.
        def mergeTreesHelper(root1, root2):
            # This is for the second situation
            if root2 == None:
                return

            # This is for the first situation
            root1.val = root1.val + root2.val

            if root1.left == None:
                # This is for the third situation
                root1.left = root2.left
            else:
                mergeTreesHelper(root1.left, root2.left)
            if root1.right == None:
                # This is for the third situation
                root1.right = root2.right
            else:
                mergeTreesHelper(root1.right, root2.right)

        mergeTreesHelper(root1, root2)

        return root1
```

## **[112] Path Sum**

Given the root of a binary tree and an integer targetSum, return true if the tree has a root-to-leaf path such that adding up all the values along the path equals targetSum.

A leaf is a node with no children.

```python
Input: root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
Output: true
```

```python
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        targetSumDic = {}
        # go through all the root, and add all the sum value into the dictionary
        def hasPathSumHelper(root, sum):
            if root.left == None and root.right == None:
                targetSumDic[sum + root.val] = 1

            if root.left != None:
                hasPathSumHelper(root.left, sum + root.val)
            
            if root.right != None:
                hasPathSumHelper(root.right, sum + root.val)

        hasPathSumHelper(root, 0)
        if targetSum in targetSumDic:
            return True
        else:
            return False
```

## **[437] Path Sum III**

Given the root of a binary tree and an integer targetSum, return the number of paths where the sum of the values along the path equals targetSum.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

```python
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```

```python
class Solution:
    def pathSum(self, root: Optional[TreeNode], targetSum: int) -> int:
        if not root:
            return 0

        numDic = {}
        self.answer = 0
        def pathSumHelper(root, sum):
            # when the sum is not equal to targetSum, try to see if the previous root sum is in dict
            # then we add the value into the answer.
            # Because there might have a lot of zeros or same numbers at the front, so we will use value instead of True or False
            if (sum + root.val - targetSum) in numDic:
                self.answer += numDic[sum + root.val - targetSum]
            # when the sum is equal to targetSum, just add 1 to answer
            if (sum + root.val) == targetSum:
                self.answer += 1

            # add the sum + root.val into dictionary
            if sum + root.val in numDic:
                numDic[sum + root.val] += 1
            else:
                numDic[sum + root.val] = 1
            # go left of the root
            if root.left != None:
                pathSumHelper(root.left, sum + root.val)
            # go right of the root
            if root.right != None:
                pathSumHelper(root.right, sum + root.val)
            # when this layer is finished, we minus 1 of the dict
            if sum + root.val in numDic:
                numDic[sum + root.val] -= 1

        # start
        pathSumHelper(root, 0)

        return self.answer
```