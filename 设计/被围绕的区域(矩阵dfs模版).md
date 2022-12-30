## 题目链接
[leetcode 130](https://leetcode.cn/problems/surrounded-regions/)

## 题目描述

给你一个 m x n 的矩阵 board ，由若干字符 'X' 和 'O' ，找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。  

m == grid.length  
n == grid[i].length  
1 <= m, n <= 300  
grid[i][j] 的值为 '0' 或 '1'  

![image](https://user-images.githubusercontent.com/58321592/210041363-0e9cd741-3ad7-4803-8fd0-adca8861994d.png)
```html
输入：board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
输出：[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
解释：被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。
```

## 解题思路

首先，将矩阵中所有与边界 O 连通的 O 都设为 # ；  
之后，将矩阵的 O 都修改为 X；  
最后，重置 # 为 O。  

```JAVA
class Solution {
    // O(mn) O(mn)
    public void solve(char[][] board) {
        int m = board.length;
        int n = board[0].length;
        // 将所有与边界O连通的O都设为#
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if ((i == 0 || i == m - 1 || j == 0 || j == n - 1) && board[i][j] == 'O') {
                    dfs(board, i, j);
                } 
            }
        }
        // 将棋盘中剩余的O改为X，将#重新设为O
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                } 
                if (board[i][j] == '#') {
                    board[i][j] = 'O';
                } 
            }
        }
    }
    // 从 (i, j) 开始把连通的 O 全部设为 #
    private void dfs(char[][] board, int i, int j) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) {
            return;
        }
        if (board[i][j] == '#' || board[i][j] == 'X') {
            return;
        }
        board[i][j] = '#';
        dfs(board, i - 1, j);
        dfs(board, i + 1, j);
        dfs(board, i, j - 1);
        dfs(board, i, j + 1);
        // 注意这里面不能回溯撤销操作，保留dfs的标记作用，这是与单词搜索回溯操作最大的差别
    }
}
```

