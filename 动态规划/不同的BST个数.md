## 题目链接

[leetcode 96](https://leetcode.cn/problems/unique-binary-search-trees/)  

## 题目描述

给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。   

![image](https://user-images.githubusercontent.com/58321592/194867551-b311461a-f46a-409c-9621-a30e4ea6215d.png)
```html
输入：n = 3
输出：5
```

## 解题思路  

**考虑以序列中每个结点作为根能够成 BST 的个数，再求和，能够成 BST 的个数只与不同值个数有关与具体值无关。**

- 状态函数
  - 设 dp[i] 表示 i 个不同的值能够组成的 BST 个数
  - 自变量范围 0 <= i <= n
  - 目标求 dp[n]
- 转移方程
  - dp[i] = Σj dp[j - 1] * dp[i - j]， 即考虑每个值作为根能够成 BST 的个数，再求和
  - 转移方程自变量范围  1 <= j <= i
  - 边界值 dp[0] = 1
- 边界值及鲁棒性
  - n == 0 或 n == 1 时是边界
  - n >= 2 走转移方程
- 空间压缩
  - dp[i] 依赖 dp[0] ~ dp[i - 1] ，无法压缩空间


```java
class Solution {
    // O(n^2) O(n)
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                // 以j作为根， 左子树中不同值结点数为 j - 1 ， 右子树不同值结点数为 i - j
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
}
```



