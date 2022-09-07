# 3. 数组中重复的数字

## 题目链接

[leetcode 704](https://leetcode.cn/problems/binary-search/)

## 题目描述

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

```html
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4

输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

## 解题思路

二分搜索秉持将有序序列中点元素nums[mid]与目标值target进行比较，每次筛选掉一半元素的原则  
基本情况：  
    如果查找区间[L, R]无效，返回-1，
    否则：
        如果nums[mid] < target, 去右半区间继续查找;
        否则如果nums[mid] > target, 去左半区间查找;
        否则返回mid
边界及鲁棒：nums为null或length=0都返回-1


```JAVA
class Solution {
    // nums不重复且有序
    // // 迭代法 O(logn) O(1)
    // public int search(int[] nums, int target) {
    //     if (nums == null || nums.length == 0) {
    //         return -1;
    //     }
    //     int left = 0, right = nums.length - 1;
    //     // 在[left, right]区间查找
    //     while (left <= right) {
    //         int mid = left + ((right - left) >> 1);
    //         if (nums[mid] < target) {
    //             left = mid + 1;
    //         } else if (nums[mid] > target) {
    //             right = mid - 1;
    //         } else {
    //             return mid;
    //         }
    //     }
    //     return -1;
    // }

    // 递归法 O(logn) O(1)
    public int search(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        return search(nums, 0, nums.length - 1, target);
    }

    private int search(int[] nums, int L, int R, int target) {
        if (L > R) {
            return -1;
        }
        if (L == R) {
            return nums[L] == target ? L : -1;
        }
        int mid = L + ((R - L) >> 1);
        if (nums[mid] < target) {
            return search(nums, mid + 1, R, target);
        }
        if (nums[mid] > target) {
            return search(nums, L, mid - 1, target);
        }
        return mid;
    }
}
```

