## 题目链接

[leetcode 215](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

## 题目描述

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。  
请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。  
你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。  
1 <= k <= nums.length <= 105

```html
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

## 解题思路

快速选择(二分 + partition)算法：  
在[L, R]范围进行切分，切分元素索引idx:  
　　若idx < n - k, 舍弃左侧元素递归去右侧切分寻找  
　　否则若idx > n - k, 舍弃右侧元素递归去左侧切分寻找  
　　否则返回当前位置元素  
时间复杂度 T(n) = T(n / 2) + O(n) => O(n)

```JAVA
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int n = nums.length;
        int L = 0, R = n - 1;
        while (L <= R) {
            int idx = partition(nums, L, R);
            if (idx < n - k) {
                L = idx + 1;
            } else if (idx > n - k) {
                R = idx - 1;
            } else {
                return nums[idx];
            }
        }
        return -1;
    }

    private int partition(int[] nums, int L, int R) {
        int temp = nums[L];
        int i = L, j = R;
        while (i < j) {
            while (i < j && nums[j] > temp) {
                j--;
            }
            if (i < j) {
                nums[i++] = nums[j];
            }
            while (i < j && nums[i] <= temp) {
                i++;
            }
            if (i < j) {
                nums[j--] = nums[i];
            }
        }
        nums[j] = temp;
        return j;
    }
}
```

