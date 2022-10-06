## 题目链接

[leetcode 51](https://leetcode.cn/problems/n-queens/)  
[leetcode 52](https://leetcode.cn/problems/n-queens-ii/)

## 题目描述

给你一个 n × n 的棋盘，要求在上边放置 n 个皇后，使任意两个皇后不能发生冲突（不能同行、同列、同对角线），  
求所有防止的方案(数)。答案中使用 Q 记录皇后，使用 . 记录空位。

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/N%E7%9A%87%E5%90%8E.png)
```html
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
```

## 解题思路  
- 回溯树  
![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E7%9A%87%E5%90%8E%E5%9B%9E%E6%BA%AF%E6%A0%91.png)
- 状态机(重点，可替代回溯树)  
在当前行的某一列尝试摆放皇后，如果其不与之前摆放的皇后冲突，则选择并递归处理下一行，否则在下一列尝试摆放皇后。如果所有行均以填充皇后，则收集结果。如果当前行所有列均以尝试过，则回退到上一行进行尝试。
![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/N%E7%9A%87%E5%90%8E%E7%8A%B6%E6%80%81%E6%9C%BA.png)
- 何时收集结果: 没有待填充皇后的行时
- 何时剪枝（提前终止）：当前行所挑选的列放置皇后会与已放置皇后冲突时
- 是否需要去重：棋盘问题，结点取值与位置绑定，天然不同

```JAVA
class Solution {
    public List<List<String>> solveNQueens(int n) {
        char[][] grid = new char[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                grid[i][j] = '.';
            }
        }
        List<List<String>> paths = new ArrayList<>();
        backtrace(grid, 0, paths);
        // 求方案树只需要返回size即可
        return paths;
    }
    // 在 grid 的第 row 行开始放置皇后，求所有可行方案
    private void backtrace(char[][] grid, int row, List<List<String>> paths) {
        if (row == grid.length) {
            List<String> path = new ArrayList<>();
            for (int i = 0; i < grid.length; i++) {
                StringBuilder sb = new StringBuilder();
                for (int j = 0; j < grid[0].length; j++) {
                    sb.append(grid[i][j]);
                }
                path.add(sb.toString());
            }
            paths.add(path);
            return;
        }
        // 尝试在 grid 的row行第 0 ~ col - 1 列放置元素
        for (int j = 0; j < grid[row].length; j++) {
            if (isValid(grid, row, j)) {
                grid[row][j] = 'Q';
                backtrace(grid, row + 1, paths);
                grid[row][j] = '.';
            }
        }
    }
    // 校验 0~row - 1 行已经放置的皇后是否与当前row，col位置放置的皇后发生冲突
    private boolean isValid(char[][] grid, int row, int col) {
        for (int i = 0; i < row; i++) {
            for (int j = 0; j < grid[i].length; j++) {
                if (grid[i][j] == '.') {
                    continue;
                }
                // 同列
                if (j == col) {
                    return false;
                }
                // 同对角线
                if (Math.abs(row - i) == Math.abs(col - j)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```



