# leetcode 数据结构入门

---

1. 两数之和
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。
你可以按任意顺序返回答案。
1. Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.

思路：1.暴力法（第一反应...要改进），具体通过两个循环，使nums[i]+nums[j]=target
2.python 字典dict 可以视为hashmap的内置实现，一边遍历一边建表

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        dicNums = {}
        for i in range(len(nums)):
            if nums[i] in dicNums:
                return dicNums[nums[i]], i
            dicNums[target - nums[i]] = i  
```


88. 合并两个有序数组
给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。
请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。
注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。
nums2 的长度为 n 。
88. Merge Sorted Array
You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.
Merge nums1 and nums2 into a single array sorted in non-decreasing order.
The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n,
where the first m elements denote the elements that should be merged,
and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

思路：1.直接合入排序
2.python内置set可以去重；python的sort()排序
3.双指针(暂时不会,大概思想是把nums2合入nums1，在0和m位置各放一个指针，然后判断左指针大于or小于右指针）

```python
class Solution:
    def merge(self, nums1, m, nums2, n):
        """
        Do not return anything, modify nums1 in-place instead.
        """
        nums1[m:] = nums2
        left = 0
        right = m
        while left <= right < m + n:
            if nums1[left] > nums1[right]:
                tmp = nums1[right]
                nums1[left + 1:right + 1] = nums1[left:right]
                nums1[left] = tmp
                left += 1
                right += 1
            else:
                left += 1
```
