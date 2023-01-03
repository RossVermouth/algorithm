## 题目链接
[leetcode 470](https://leetcode.cn/problems/implement-rand10-using-rand7/)

## 题目描述

给定 rand7() 等概率生成 1 ~ 7 内随机整数， 构造 rand10()

## 解题思路

拒绝采样：生成不满足要求的数字拒绝，直到生成符合要求的数字

rand7 => rand49 => rand40 => rand10

rand40 : P(X == 1) = 1 / 49 + 9 / 49 * 1 / 49 + (9 / 49) ^ 2 * 1 / 49 + ... = 1 / 40 

```JAVA
/**
 * The rand7() API is already defined in the parent class SolBase.
 * public int rand7();
 * @return a random integer in the range 1 to 7
 */
class Solution extends SolBase {
    // rand7 [1,..., 7] => 7 * rand7 - rand7 + 1 [1,..., 49]
    // rand49 -> reject 41~49 得 rand40
    // rand40 % 10 + 1 即 rand10, 被接受的概率更大
    public int rand10() {
        int num = 7 * rand7() - rand7() + 1;
        while (num > 40) {
            num = 7 * rand7() - rand7() + 1;
        }
        return num % 10 + 1;
    }
}
```

