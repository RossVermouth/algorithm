## 题目链接
[leetcode 560](https://leetcode.cn/problems/subarray-sum-equals-k/)

## 题目描述

给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的连续子数组的个数 。  

1 <= nums.length <= 2 * 10^4

```html
输入：nums = [1,2,3], k = 3
输出：2
```

## 解题思路

和为k的题目，因为 nums[i] 有正有负，无法单调收缩窗口，故不能使用滑动窗口，应该使用前缀和。  

prefix[i] = nums[0] + ... + nums[i]  

prefix[i] = (prefix[i] - k) + k  

即以 nums[i] 结尾的和为 k 的连续子数组个数 等于 在 nums[i] 及其之前，以 prefix[i] - k 作为前缀和的子数组个数  
 
```JAVA
class Solution {
    // O(n) O(n)
    public int subarraySum(int[] nums, int k) {
        int res = 0;
        int n = nums.length;
        Map<Integer, Integer> map = new HashMap<>();
        // 和为0的子数组个数初始化为1
        map.put(0, 1);
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            // 关键在于：以nums[i]结尾的和为k的子数组个数 <=> 在 nums[i] 及其之前，以 prefix[i] - k 作为前缀和的子数组个数
            if (map.containsKey(sum - k)) {
                res += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return res;
    }
}
```

