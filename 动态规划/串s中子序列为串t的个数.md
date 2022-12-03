## 题目链接

[leetcode 115](https://leetcode.cn/problems/distinct-subsequences/)  

## 题目描述

给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。  

0 <= s.length, t.length <= 500  
s 和 t 由小写英文字母组成  

```html
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

## 解题思路  

本质是只有删除操作的编辑距离。  

- 状态函数
  - 设 dp[i][j] 表示将 s[0 ~ i] 删除字符转为 t[0 ~ j] 的可能方法数
  - 自变量范围 0 <= i <= m - 1 ,  0 <= j <= n - 1 
  - 目标求 dp[m - 1][n - 1]
- 转移方程
  - dp[i][j] = s[i] == t[j] ? dp[i - 1][j - 1] + dp[i - 1][j] : dp[i - 1][j]
  - i >= 1 ,  j >= 1
  - **两串前加相同哨兵** 边界值 dp[0][0] = 1, dp[0][j] = 0, dp[i][0] = 1
- 边界值及鲁棒性
  - m n 为 0 即加哨兵后均为 1 时，dp[0][0] = 1 即为结果
  
```java
class Solution {
    // O(mn) O(mn)
    public int numDistinct(String s, String t) {
        s = "#" + s;
        t = "#" + t;
        int m = s.length();
        int n = t.length();
        int[][] dp = new int[m][n];
        dp[0][0] = 1;
        for (int j = 1; j < n; j++) {
            dp[0][j] = 0;
        }
        for (int i = 1; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = s.charAt(i) == t.charAt(j) ? dp[i - 1][j - 1] + dp[i - 1][j] : dp[i - 1][j];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
