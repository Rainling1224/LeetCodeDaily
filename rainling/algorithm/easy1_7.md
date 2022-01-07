# leetcode 数据结构 day4

---

566. 重塑矩阵
在 MATLAB 中，有一个非常有用的函数 reshape ，它可以将一个 m x n 矩阵重塑为另一个大小不同（r x c）的新矩阵，但保留其原始数据。
给你一个由二维数组 mat 表示的 m x n 矩阵，以及两个正整数 r 和 c ，分别表示想要的重构的矩阵的行数和列数。
重构后的矩阵需要将原始矩阵的所有元素以相同的 行遍历顺序 填充。
如果具有给定参数的 reshape 操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。
566. Reshape the Matrix
In MATLAB, there is a handy function called reshape which can reshape an m x n matrix into a new one with a different size r x c keeping its original data.
You are given an m x n matrix mat and two integers r and c representing the number of rows and the number of columns of the wanted reshaped matrix.
The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were.
If the reshape operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

思路：1.按行列遍历每个元素，重新c个元素成一行，一共有r行即可

2.直接使用 NumPy 的 reshape() 函数

```python
#1
class Solution:
    def matrixReshape(self, nums: List[List[int]], r: int, c: int) -> List[List[int]]:
        if len(nums) * len(nums[0]) != r * c:
            return nums
        result = []
        path = []
        for words in nums:
            for word in words:
                path.append(word)
                if len(path) == c:
                    result.append(path)
                    path = []
        return result


#2
class Solution:
    def matrixReshape(self, nums: List[List[int]], r: int, c: int) -> List[List[int]]:
        import numpy as np
        row, col = len(nums), len(nums[0])
        if row * col != r * c: return nums
        return np.reshape(nums, (r, c)).tolist()

```

118. 杨辉三角
给定一个非负整数 numRows，生成「杨辉三角」的前 numRows 行。
在「杨辉三角」中，每个数是它左上方和右上方的数的和。
118. Pascal's Triangle
Given an integer numRows, return the first numRows of Pascal's triangle.
In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

思路：1.暴力解法

除了行首尾的两个1以外，第n行第i个数=第n-1行第i个数+第n-1行第i-1个数(n>2)，即res[n][i]=res[n-1][i]+res[n-1][i-1]。且第n行有n个数，即len(ROWn)=n。
而每一行是左右对称的，即row[j]=row[n-1-j]，n为行数。那么可将内循环范围设为[1,math.ceil(n/2)]减小内循环范围。

2.动态规划

头尾为1，中间的第j个为上一层的第j-1个和j个的和，即：
第i行lst[j] = ans[i-1][j-1] + ans[i-1][j]


```python
#1
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        if numRows==1: return [[1]]
        if numRows==2: return [[1],[1,1]]
        res=[[1],[1,1]]
        for i in range(3,numRows+1):
            tmp=[1]*i
            k=math.ceil(i/2)
            for j in range(1,k):
                tmp[i-1-j]=tmp[j]=res[-1][j]+res[-1][j-1]
            res.append(tmp)
        return res


#2
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        ans = [[1]]
        for i in range(1, numRows):
            lst = [0 for _ in range(i+1)]
            lst[0], lst[-1] = 1, 1
            for j in range(1,i):  
                lst[j] = ans[i-1][j-1] + ans[i-1][j]
            ans.append(lst)
        return ans

```
