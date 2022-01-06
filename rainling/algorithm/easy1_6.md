# leetcode 数据结构 day3

---

350. 两个数组的交集 II
给你两个整数数组 nums1 和 nums2 ，请你以数组形式返回两数组的交集。
返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。
350. Intersection of Two Arrays II
Given two integer arrays nums1 and nums2, return an array of their intersection. 
Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

思路：1.使用交集

2.hash(有种万事都可以hash的感觉)

3.字典

4.直接统计每个元素次数(数组小的话可以使用)

```python
#2
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        convene = {}
        if len(nums1) > len(nums2): 
            tmp = nums1 # 如果num1元素大于num2，则交换两个数组的值
            nums1 = nums2 
            nums2 = tmp 
        for num in nums1: # 使用较小的数组来生成hash表，以节省空间
            if num in convene:
                convene[num] += 1 
                continue 
            convene[num] = 1
        result = []
        # 遍历较长的数组，如果元素在hash表里存在，且个数大于0，则说明该元素是交集
        for num in nums2: 
            if num in convene and convene[num] > 0:
                result.append(num)
                convene[num] -= 1 
        return result


#3
class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        m_1, m_2 = {}, {}
        for i in nums1:
            m_1[i] = 0
        for i in nums2:
            m_2[i] = 0
        for i in nums1:
            m_1[i] += 1
        for i in nums2:
            m_2[i] += 1
        ll = [] 
        for i in set(nums1).intersection(nums2):
                ll += [i] * min(m_1[i], m_2[i])
        return ll

#4
from collections import Counter
def intersection(nums1,nums2):
    count1 = Counter(nums1)
    count2 = Counter(nums2)
    intersect = []
    for key in count1:
        if key in count2:
            intersect+=[key]*min(count1[key],count2[key])
    return intersect



```


121. 买卖股票的最佳时机
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。
你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。
返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。
121. Best Time to Buy and Sell Stock
You are given an array prices where prices[i] is the price of a given stock on the ith day.
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.


思路：1.i选定后只能增加，循环判断i和j=i+1的差值

2.贪心，取最左最小值，取最右最大值

3.动态规划
`解题思路
到最后交易结束时，一共会有3种状态：

dp0：一直不买
dp1：只买了一次
dp2：买了一次，卖了一次
初始化3种状态：

dp0 = 0
dp1 = - prices[0]
dp2 = float("-inf")
因为第一天不可能会有dp2状态，因此将dp2置为负无穷
(Java中置为int的下边界)
对3种状态进行状态转移：

dp0 = 0
一直为0
dp1 = max(dp1, dp0 - prices[i])
前一天也是dp1状态，或者前一天是dp0状态，今天买入一笔变成dp1状态
dp2 = max(dp2, dp1 + prices[i])
前一天也是dp2状态，或者前一天是dp1状态，今天卖出一笔变成dp2状态
`

```python

#2
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        low = float("inf")
        result = 0
        for i in range(len(prices)):
            low = min(low, prices[i]) #取最左最小价格
            result = max(result, prices[i] - low) #直接取最大区间利润
        return result


#3
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp0 = 0             # 一直不买
        dp1 = - prices[0]   # 只买了一次
        dp2 = float('-inf') # 买了一次，卖了一次

        for i in range(1, len(prices)):
            dp1 = max(dp1, dp0 - prices[i])
            dp2 = max(dp2, dp1 + prices[i])
        return max(dp0, dp2)
```
