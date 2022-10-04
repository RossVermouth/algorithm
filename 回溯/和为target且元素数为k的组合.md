## 题目链接

[leetcode 216](https://leetcode.cn/problems/combination-sum-iii/)

## 题目描述

找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：  

只使用数字 1 到 9     
每个数字最多使用一次  

返回所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。


```html
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。

输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
解释:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
没有其他符合的组合了。

输入: k = 4, n = 1
输出: []
解释: 不存在有效的组合。
在[1,9]范围内使用4个不同的数字，我们可以得到的最小和是1+2+3+4 = 10，因为10 > 1，没有有效的组合
```

## 解题思路

- 何时收集结果: 当组合中元素数目为 k 且和为 target 时
- 何时剪枝（提前终止）：当元素个数超过 k 或和超过 target 或 剩余元素都不够填充元素时
- 是否需要去重：不需要去重，可选元素序列中的值都是唯一的

```JAVA
class Solution {
    // O((n, k) * k) O(k)
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> paths = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        backtrace(1, 9, k, 0, n, path, paths);
        return paths;
    }
    // 在[L, R]中寻找元素数为 k 且和为 target 的组合
    private void backtrace(int L, int R, int k, int curSum, int target, List<Integer> path, List<List<Integer>> paths) {
        // 结果收集条件
        if (path.size() == k && curSum == target) {
            paths.add(new ArrayList<>(path));
            return;
        }
        // 剪枝条件
        if (curSum >= target || path.size() >= k || R - L + 1 < k - path.size()) {
            return;
        }
        // 从剩余可选元素中挑元素
        for (int i = L; i <= R; i++) {
            // 选择
            path.add(i);
            curSum += i;
            // 递归处理，不可重复选所以左边界后推
            backtrace(i + 1, R, k, curSum, target, path, paths);
            // 回溯
            curSum -= i;
            path.remove(path.size() - 1);
        }
    }
}
```



