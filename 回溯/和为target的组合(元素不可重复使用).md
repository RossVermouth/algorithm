## 题目链接

[leetcode 40](https://leetcode.cn/problems/combination-sum-ii/)

## 题目描述

在一个可能包含重复元素序列中，求和为 target 的所有**不同**组合，**每个元素最多使用一次**。  



```html
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

输入: candidates = [2,5,2,1,2], target = 5,
输出:
[
[1,2,2],
[5]
]
```

## 解题思路

- 何时收集结果: 当组合中元素和为 target 时，即收集结果  
- 何时剪枝（提前终止）：当元素和超过 target 时，即剪枝
- 是否需要去重：层级去重，排序 + set避免出现重复组合

```JAVA
class Solution {
    // O(2^n * n) O(n)
    // 在39得基础上修改一个元素仅能使用一次，并且加上层级去重
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> paths = new ArrayList<>();
        List<Integer> path = new ArrayList<>();
        Arrays.sort(candidates);
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
        Set<Integer> set = new HashSet<>();
        for (int i = L; i <= R; i++) {
            if (set.contains(candidates[i])) {
                continue;
            }
            set.add(candidates[i]);
            path.add(candidates[i]);
            curSum += candidates[i];
            backtrace(candidates, i + 1, R, curSum, target, path, paths);
            curSum -= candidates[i];
            path.remove(path.size() - 1);
        }
    }
}
```



