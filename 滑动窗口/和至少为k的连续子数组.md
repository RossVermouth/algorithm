## 题目链接

[leetcode 209](https://leetcode.cn/problems/minimum-size-subarray-sum/)

## 题目描述

给定一个含有 n 个正整数的数组和一个正整数 target 。  

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。  

```html
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

## 解题思路

变长滑动窗口求满足某性质的最短连续子序列性质。  

[left, right) 定义窗口，right 是下一个待进入窗口的元素位置。  

当 right 未越界时，将 right 位置元素划入窗口，当窗口元素满足结果收集条件时，收集结果并尝试收缩窗口。  

```JAVA
class Solution {
    // O(n) O(1)
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int res = Integer.MAX_VALUE;
        int sum = 0;
        int left = 0, right = 0;
        while (right < n) {
            sum += nums[right++];
            while (sum >= target && left <= right) {
                res = Math.min(res, right - left);
                sum -= nums[left++];
            }
        }
        return res == Integer.MAX_VALUE ? 0 : res;
    }
}
```

