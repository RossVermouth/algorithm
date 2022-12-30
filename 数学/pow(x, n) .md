## 题目链接
[leetcode 50](https://leetcode.cn/problems/powx-n/)

## 题目描述

实现 pow(x, n) ，即计算 x 的整数 n 次幂函数（即，x ^ n ）。  

-100.0 < x < 100.0  
-2^31 <= n <= 2^31-1  
n 是一个整数  
-10^4 <= x^n <= 10^4  

```html
输入：x = 2.00000, n = 10
输出：1024.00000

输入：x = 2.00000, n = -2
输出：0.25000
解释：2-2 = 1/22 = 1/4 = 0.25
```

## 解题思路

x != 0 且 n >= 0 时走快速幂：  

即 n == 0, 返回 1 ；n == 1， 返回 x ；否则计算 x ^ (n / 2) 快速求解。

x == 0, 直接返回 0 ；  

否则根据 n 的正负决定是否取倒数。  

快速幂的 n 应为 long，否则可能取负数会溢出。  

边界 case 太多。



```JAVA
class Solution {
    public double myPow(double x, int n) {
        if (Math.abs(x) < 1e-6) {
            return 0.0;
        }
        return n >= 0 ? myPowWithPositive(x, n) : 1.0 / myPowWithPositive(x, -n);
    }
    // x != 0 n >= 0 快速幂模版
    private double myPowWithPositive(double x, long n) {
        if (n == 0) {
            return 1.0;
        }
        if (n == 1) {
            return x;
        }
        double halfPow = myPowWithPositive(x, n / 2);
        return n % 2 == 0 ? halfPow * halfPow : x * halfPow * halfPow;
    }
}
```

