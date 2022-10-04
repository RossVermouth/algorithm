## 题目链接

[leetcode 39](https://leetcode.cn/problems/combination-sum/)

## 题目描述

在一个无重复元素序列中，求和为 target 的所有组合，每个元素可以使用多次。  



```html
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。

输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]

输入: candidates = [2], target = 1
输出: []
```

## 解题思路

- 何时收集结果: 当组合中元素和为 target 时，即收集结果  
- 何时剪枝（提前终止）：当元素和超过 target 时，即剪枝
- 是否需要去重：数组中无重复元素，不需要进行层级去重

```JAVA
class Solution {
    // O(S) S为所有可行解长度和 O(target)
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> paths = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        backtrace(candidates, 0, candidates.length - 1, 0, target, path, paths);
        return paths;
    }

    private void backtrace(int[] candidates, int L, int R, int curSum, int target, 
                            List<Integer> path, List<List<Integer>> paths) {
        if (curSum == target) {
            paths.add(new ArrayList<>(path));
            return;
        }
        if (curSum > target) {
            return;
        }
        for (int i = L; i <= R; i++) {
            path.add(candidates[i]);
            curSum += candidates[i];
            // 一个元素是可以重复选取得，故选完之后可以接着选
            backtrace(candidates, i, R, curSum, target, path, paths);
            curSum -= candidates[i];
            path.remove(path.size() - 1);
        }
    }
}
```



