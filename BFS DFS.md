# Search BFS, DFS

bfs
* [[69] Sqrt(x)](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#69-sqrtx)
* [[279] Perfect Squares](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#279-perfect-squares)

dfs
* [[695] Max Area of Island](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#695-max-area-of-island)
* [[547] Number of Provinces](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#547-number-of-provinces)
* [[130] Surrounded Regions](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#130-surrounded-regions)
* [[417] Pacific Atlantic Water Flow](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#417-pacific-atlantic-water-flow)

backtracking
* [[93] Restore IP Addresses](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#93-restore-ip-addresses)
* [[79] Word Search](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#79-word-search)
* [[257] Binary Tree Paths](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#257-binary-tree-paths)
* [[77] Combinations](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#77-combinations)
* [[216] Combination Sum III](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#216-combination-sum-iii)
* [[90] Subsets II](https://github.com/shaojim12/Leetcode-notes/blob/master/BFS%20DFS.md#90-subsets-ii)
* [[131] Palindrome Partitioning]()
* [[37] Sudoku Solver]()
* [[51] N-Queens]()

## **[69] Sqrt(x)**

Given an n x n binary matrix grid, return the length of the shortest clear path in the matrix. If there is no clear path, return -1.

A clear path in a binary matrix is a path from the top-left cell (i.e., (0, 0)) to the bottom-right cell (i.e., (n - 1, n - 1)) such that:

All the visited cells of the path are 0.
All the adjacent cells of the path are 8-directionally connected (i.e., they are different and they share an edge or a corner).
The length of a clear path is the number of visited cells of this path.

```python
Input: grid = [[0,1],[1,0]]
Output: 2
Input: grid = [[0,0,0],[1,1,0],[1,1,0]]
Output: 4
Input: grid = [[1,0,0],[1,1,0],[1,1,0]]
Output: -1
```

```python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        
        max_row = len(grid) - 1
        max_column = len(grid[0]) - 1
        # get other eight directions
        direction = [(-1, -1), (-1, 0), (-1, 1), (0, -1), (0, 1), (1, -1), (1, 0), (1, 1)]
        
        def get_nei(row, col):
            # return the neighbor that is zero, it means that it is an available path
            neibor = []
            for dir_row, dir_col in direction:
                new_row = row + dir_row
                new_col = col + dir_col
                if 0 <= new_row and new_row <= max_row and 0 <= new_col and new_col <= max_column:
                    if grid[new_row][new_col] == 0:
                        neibor.append((new_row, new_col))

            return neibor
            

        if grid[0][0] != 0 or grid[max_row][max_column] != 0:
            return -1

        queue = deque()
        queue.append((0, 0))
        grid[0][0] = 1

        while queue:
            row, col = queue.popleft()
            distance = grid[row][col]
            if row == max_row and col == max_column:
                # return the last distance when we reach the final point
                return distance
            for (nei_row, nei_col) in get_nei(row, col):
                grid[nei_row][nei_col] = distance + 1 # add one to the distance
                queue.append((nei_row, nei_col)) # add all the adjacent point to queue
        
        return -1
```


## **[279] Perfect Squares**

Given an integer n, return the least number of perfect square numbers that sum to n.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

```python
Input: n = 12
Output: 3
Explanation: 12 = 4 + 4 + 4.
Input: n = 13
Output: 2
Explanation: 13 = 4 + 9.
```

```python
class Solution:
    def numSquares(self, n: int) -> int:        
        if not n:
            return 0

        squares = [i*i for i in range(1, int(n**0.5)+1)] # get the square that fit our n
        distance, queue, s_queue = 1, [n], set() # set() can remove the duplicate numbers

        while queue:
            for node in queue:
                for square in squares:
                    if (node - square) == 0: return distance
                    if (node - square) < 0: break

                    s_queue.add(node - square) # we add all the possible numbers in set

            queue = s_queue 
            s_queue = set() # set s_queue to empty set
            distance += 1
```

## **[695] Max Area of Island**

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

```python
Input: grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
Output: 6
Explanation: The answer is not 11, because the island must be connected 4-directionally.
```

```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        row, column = len(grid) - 1, len(grid[0]) - 1

        def dfs(i, j):
            if i >=0 and i<= row and j >=0 and j<= column and grid[i][j] == 1:
                grid[i][j] = 0
                return 1 + dfs(i-1, j) + dfs(i+1, j) + dfs(i, j-1) + dfs(i, j+1)
            return 0

        max_island = 0
        for i in range(row+1):
            for j in range(column+1):
                max_island = max(dfs(i, j), max_island)

        return max_island
```

## **[547] Number of Provinces**

There are n cities. Some of them are connected, while some are not. If city a is connected directly with city b, and city b is connected directly with city c, then city a is connected indirectly with city c.

A province is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an n x n matrix isConnected where isConnected[i][j] = 1 if the ith city and the jth city are directly connected, and isConnected[i][j] = 0 otherwise.

Return the total number of provinces.

```python
Input: isConnected = [[1,1,0],[1,1,0],[0,0,1]]
Output: 2
```

```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        provinces = 0
        isVisited = [False for i in range(len(isConnected))]

        def dfs(i):
            isVisited[i] = True
            for j in range(len(isConnected)):
                if isVisited[j] == False and isConnected[i][j] == 1:
                    dfs(j)


        for i in range(len(isConnected)):
            if isVisited[i] == False:
                provinces += 1
                dfs(i)

        return provinces
```

## **[130] Surrounded Regions**

Given an m x n matrix board containing 'X' and 'O', capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

```python
Input: board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
Output: [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
Explanation: Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
```

```python
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        total_row, total_column = len(board) - 1, len(board[0]) - 1 
        isVisited = [[False for i in range(len(board[0]))] for j in range(len(board))]
        direction = [(-1, 0), (1, 0), (0, 0), (0, -1), (0, 1)]
        connect_point = []
        flip = [True]

        def dfs(i, j):
            # visit the point that is not visit
            if isVisited[i][j] == False:
                isVisited[i][j] = True
                # find connected point
                for row, col in direction:
                    if (i + row) >= 0 and (i+row) <= total_row and (j + col) >= 0 and (j+ col) <= total_column:
                        if board[i+row][j+col] == "O":
                            # check whether it is boarder
                            if (i + row) == 0 or (i+row) == total_row or (j + col) == 0 or (j+ col) == total_column:
                                flip[0] = False # if it is boarder, don't flip the card
                            connect_point.append((i+row, j+col))
                            dfs(i+row, j+col)
        
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == "O" and isVisited[i][j] == False:
                    connect_point.clear() # reset connect_point
                    flip[0] = True # reset the flip flag
                    dfs(i, j)
                    if flip[0] == True:
                        # flip all the card that is not connect to boarder
                        for flip_row, flip_col in connect_point:
                            board[flip_row][flip_col] = "X"
```

## **[417] Pacific Atlantic Water Flow**

There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

```python
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
```

```python
class Solution:
    def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
        total_row, total_column = len(heights) - 1, len(heights[0]) - 1
        IslandToOcean = [[[0, 0] for i in range(len(heights[0]))] for j in range(len(heights))]
        direction = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        results = []

        # We divide algorithm into two steps, 
        # one is to find the pacific ocean, which start from left and top, 
        # when the neighbor number is greater, we set IslandToOcean[i][j][0] to one.
        def dfs_pacific(i, j):
            for row, col in direction:
                if (i + row) >= 0 and (i+row) <= total_row and (j + col) >= 0 and (j+ col) <= total_column:
                    if heights[i+row][j+col] >= heights[i][j]:
                        if IslandToOcean[i+row][j+col][0] == 0:
                            IslandToOcean[i+row][j+col][0] = 1
                            dfs_pacific(i+row, j+col)

        # The other ocean alantic also do the same thing, 
        # we start from right bottom, when the neighbor number is greater, 
        # we set IslandToOcean[i][j][1] to one.
        def dfs_alantic(i, j):
            for row, col in direction:
                if (i + row) >= 0 and (i+row) <= total_row and (j + col) >= 0 and (j+ col) <= total_column:
                    if heights[i+row][j+col] >= heights[i][j]:
                        if IslandToOcean[i+row][j+col][1] == 0:
                            IslandToOcean[i+row][j+col][1] = 1
                            dfs_alantic(i+row, j+col)   

        # code start from here
        for i in range(len(heights)):
            for j in range(len(heights[0])):
                if i == 0 or j == 0:
                    if IslandToOcean[i][j][0] == 0:
                        IslandToOcean[i][j][0] = 1
                        dfs_pacific(i, j)
                if i == total_row or j == total_column:
                    if IslandToOcean[i][j][1] == 0:
                        IslandToOcean[i][j][1] = 1
                        dfs_alantic(i, j)

        # find the point that belongs to both pacific and alantic oceans
        for i in range(len(heights)):
            for j in range(len(heights[0])):
                if IslandToOcean[i][j][0] == 1 and IslandToOcean[i][j][1] == 1:
                    results.append([i, j])
        
        return results
```

## **[93] Restore IP Addresses**

Given a string s containing only digits, return all possible valid IP addresses that can be obtained from s. You can return them in any order.

A valid IP address consists of exactly four integers, each integer is between 0 and 255, separated by single dots and cannot have leading zeros. For example, "0.1.2.201" and "192.168.1.1" are valid IP addresses and "0.011.255.245", "192.168.1.312" and "192.168@1.1" are invalid IP addresses. 

```python
Input: s = "25525511135"
Output: ["255.255.11.135","255.255.111.35"]
Input: s = "0000"
Output: ["0.0.0.0"]
```

```python
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        res = []
        self.dfs(s, 0, "", res)
        return res

    def dfs(self, s, idx, path, res):
        # when there is too much number left, 
        # we return it 
        if idx > 4:
            return

        # if the index is full and there is no number inside s, 
        # it means that it is a valid IP
        if idx == 4 and not s:
            res.append(path[:-1])
            return

        # only add the number of "0", 
        # or the numbers that first letter is non-zero 
        # and between 0~255
        for i in range(1, len(s)+1):
            if s[:i] == "0" or (s[0] != "0" and 0 < int(s[:i]) <= 255):
                self.dfs(s[i:], idx+1, path + s[:i] + ".", res)
```

## **[79] Word Search**

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

```python
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
Output: true
Input: board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
Output: false
```

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        self.direction = [(-1, 0), (1, 0), (0, -1), (0, 1)]
        self.answer = False
        for i in range(len(board)):
            for j in range(len(board[0])):
                if board[i][j] == word[0]:
                    self.dfs(board, [], 1, word, i, j)

        return self.answer

    def dfs(self, board, used_list, word_idx, word, now_row, now_col):
        # add the current point to used list to prevent duplicate use
        used_list.append((now_row, now_col))
        # when the word index is enough, it means that we find all the letters
        if word_idx == len(word):
            self.answer = True
            return 

        # iterate through four direction
        for row, col in self.direction:
            curr_row, curr_col = now_row + row, now_col + col
            if 0 <= curr_row <= (len(board) - 1) and 0 <= (curr_col) <= (len(board[0]) - 1):
                if board[curr_row][curr_col] == word[word_idx] and (curr_row, curr_col) not in used_list:
                    # the list will inherit the previous one, 
                    # so we have to set it as used_list[:word_idx+1] instead used_list
                    self.dfs(board, used_list[:word_idx+1], word_idx+1, word, curr_row, curr_col)
```

## **[257] Binary Tree Paths**

Given the root of a binary tree, return all root-to-leaf paths in any order.

A leaf is a node with no children.

```python
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
Input: root = [1]
Output: ["1"]
```

```python
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        if not root:
            return []
        res = []
        self.dfs(root, res, [], 1)
        answer = []
        
        # modify the style 
        for list in res:
            s = ""
            for value in list:
                s = s + str(value) + "->"
            answer.append(s[:-2])
  
        return answer

    def dfs(self, node, res, path, idx):
        path.append(node.val)
        # when both right and left is None, it means we found the leaf point
        if node.left == None and node.right == None:
            res.append(path)

        # when left or right has value, we go through the node
        if node.left:
            self.dfs(node.left, res, path[:idx], idx+1)
        
        if node.right:
            self.dfs(node.right, res, path[:idx], idx+1)
```

## **[77] Combinations**

Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].

You may return the answer in any order.

```python
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        res = []
        self.k = k
        # start from different number
        for i in range(1, n+2-k):
            self.dfs(res, i, n+1, [], 0)

        return res

    def dfs(self, res, idx, max_num, combine, total_num):
        combine.append(idx)
        # when the length is enough, we return the list
        if len(combine) == self.k:
            res.append(combine)
            return

        # iterate the rest number
        for i in range(idx+1, max_num):
            self.dfs(res, i, max_num, combine[:total_num+1], total_num+1)
```

## **[216] Combination Sum III**

Find all valid combinations of k numbers that sum up to n such that the following conditions are true:

Only numbers 1 through 9 are used.
Each number is used at most once.
Return a list of all possible valid combinations. The list must not contain the same combination twice, and the combinations may be returned in any order.

```python
Input: k = 3, n = 7
Output: [[1,2,4]]
Explanation:
1 + 2 + 4 = 7
There are no other valid combinations.
```

```python
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        res = []
        # the first element that we start
        for i in range(1, 11-k):
            self.dfs(res, 1, k, [], n-i, i)
        return res
    # res = result, idx = how many numbers we used
    # total_num = k, combine_num = each pair of the numbers
    # residual_num = numbers left, curr_num = iterate from 1 to 9
    def dfs(self, res, idx, total_num, combine_num, residual_num, curr_num):
        combine_num.append(curr_num)
        # when both residual number and index equal to zero
        if residual_num == 0 and idx == total_num:
            res.append(combine_num)
            return
        # when the number is enough and residual number is greater than zero
        elif (total_num - idx) < (10 - curr_num) and residual_num > 0:
            for i in range(curr_num+1, 10):
                self.dfs(res, idx+1, total_num, combine_num[:idx], residual_num-i, i)
        # otherwise return everything
        else:
            return 
```

## **[90] Subsets II**

Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

```python
Input: nums = [1,2,2]
Output: [[],[1],[1,2],[1,2,2],[2],[2,2]]
```

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []
        nums.sort() # sort the nums
        self.dfs(res, -1, nums, -1, [])
        return res

    # res = result list, idx = count numbers that are in combines
    # nums = nums, num_idx = store the index of where the position in the nums
    # combine = store all the different combines
    def dfs(self, res, idx, nums, num_idx, combines):
        # skip the first one 
        if idx != -1:
            combines.append(nums[num_idx])
        # add the non-duplicate list to result
        if combines not in res:
            res.append(combines)
        # iterate through the nums
        for i in range(num_idx+1, len(nums)):
            self.dfs(res, idx+1, nums, i, combines[:idx+1])
```

## **[131] Palindrome Partitioning**

Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

A palindrome string is a string that reads the same backward as forward.

```python
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        res = []
        self.dfs(res, s, 0, [], 0)
        return res


    def dfs(self, res, string, start, currentList, idx):
        # when start is equal to the end of the string, 
        # we add the sting to the result
        if start >= len(string):
            res.append(currentList)

        for i in range(start, len(string)):
            # check whether the string is palindrome
            if self.isPalindrome(string, start, i):
                # add the palindrome string into the curr list
                currentList.append(string[start:i+1])
                self.dfs(res, string, i+1, currentList, idx+1)
                # take the previous list to complete dfs
                currentList = currentList[:idx]


    def isPalindrome(self, string, left, right):
        while left < right:
            if string[left] != string[right]:
                return False
            left += 1
            right -= 1

        return True
```

## **[37] Sudoku Solver**

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

Each of the digits 1-9 must occur exactly once in each row.
Each of the digits 1-9 must occur exactly once in each column.
Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.
The '.' character indicates empty cells.


```python
class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        self.board = board
        self.digits = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]
        self.solve()

    def solve(self):
        # find the empty cell
        row, col = self.findEmpty()
        if row == -1 and col == -1:
            return True
        
        # check which digit fits in the box, then iterater through next empty cell
        # if no digit is possible to fit in the cell, we will go back to the previous cell
        # until all the cell fit in the digit, then all the solve() will return True
        for digit in self.digits:
            if self.checkRow(digit, row) and self.checkColumn(digit, col) and self.checkBoxes(digit, row, col):
                self.board[row][col] = digit
                if self.solve():
                    return True
            self.board[row][col] = "."

        return False

    def findEmpty(self):
        # search for the empty cell
        for i in range(9):
            for j in range(9):
                if self.board[i][j] == ".":
                    return i, j

        return -1, -1

    def checkRow(self, digit, row):
        for i in range(9):
            if self.board[row][i] == digit:
                return False
        return True

    def checkColumn(self, digit, col):
        for i in range(9):
            if self.board[i][col] == digit:
                return False
        return True

    def checkBoxes(self, digit, row, col):
        box_row = row // 3
        box_col = col // 3
        for i in range(3):
            for j in range(3):
                if self.board[box_row*3+i][box_col*3+j] == digit:
                    return False
        return True
```

## **[51] N-Queens**

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

```python
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        self.length = n
        self.board = [["." for i in range(n)] for j in range(n)]
        self.answer = []
        self.solved()
        return self.answer

    def solved(self):
        row = self.find_next_valid_row()
        if row == -1:
            self.answer.append([])
            # to combine all the list to a string
            for j in range(len(self.board[0])):
                self.answer[-1].append("".join(self.board[j]))
            return 
        
        # iterate through every col
        for col in range(self.length):
            if self.checkQueen(row, col):
                self.board[row][col] = "Q"
                self.solved()
                self.board[row][col] = "."
        return 

    def find_next_valid_row(self):
        # get the next row
        for i in range(self.length):
            if "Q" not in self.board[i]:
                return i
        # if complete, then return -1
        return -1

    def checkQueen(self, row, col):
        # check the top one 
        if (row-1) >= 0:
            if self.board[row-1][col] == "Q":
                return False
        
        for i in range(row):
            # check the row
            if self.board[i][col] == "Q":
                return False
            left_col = col - (i+1)
            right_col = col + (i+1)
            diagonal_row = row - (i+1)
            # check diagonal
            if left_col >= 0 and self.board[diagonal_row][left_col] == "Q":
                return False
            if right_col < self.length and self.board[diagonal_row][right_col] == "Q":
                return False

        return True
```
