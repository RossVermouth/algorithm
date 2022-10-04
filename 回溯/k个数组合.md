## 题目链接

[leetcode 77](https://leetcode.cn/problems/combinations/)

## 题目描述

给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。  

1 <= k <= n  

```html
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

输入：n = 1, k = 1
输出：[[1]]
```

## 解题思路

- 回溯树：  
![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/k%E4%B8%AA%E6%95%B0%E7%BB%84%E5%90%88.png)  
- 何时收集结果: 当组合中元素个数为 k 时，即收集结果  
- 何时剪枝（提前终止）：当剩余的可选元素数目都不够填补需要元素时
- 是否需要去重：可选元素序列中无重复元素，求组合天然保证唯一，不需要进行层级去重

```JAVA
class Solution {
    // O((n, k) * k) O(k)
    // 在组合中保留所有元素个数为k的结果即可
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> paths = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        backtrace(1, n, k, path, paths);
        return paths;
    }

    private void backtrace(int L, int R, int k, List<Integer> path, List<List<Integer>> paths) {
        if (path.size() == k) {
            paths.add(new ArrayList<>(path));
            return;
        }
        if (R - L + 1 < k - path.size()) {
            return;
        }
        for (int i = L; i <= R; i++) {
            path.add(i);
            backtrace(i + 1, R, k, path, paths);
            path.remove(path.size() - 1);
        }
    }
}
```



