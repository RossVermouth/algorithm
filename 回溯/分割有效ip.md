## 题目链接

[leetcode 93](https://leetcode.cn/problems/restore-ip-addresses/)

## 题目描述

将一个字符串使用 "." 分割成有效的 IPv4 地址，求所有有效的分割方案。  

```html
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]

输入：s = "0000"
输出：["0.0.0.0"]
```

## 解题思路

- 何时收集结果: 叶子结点，即待分割序列为空且分割出4个词
- 何时剪枝（提前终止）：序列没用完但是已经分割出4个词就提前终止 
- 是否需要去重：分割问题，必定无重复

```JAVA
class Solution {
    // O(3^4 * S) O(1) 
    public List<String> restoreIpAddresses(String s) {
        List<String> paths = new ArrayList<>();
        List<String> path = new ArrayList<>();
        backtrace(s, 0, s.length() - 1, path, paths);
        return paths;
    }

    private void backtrace(String s, int L, int R, List<String> path, List<String> paths) {
        if (L > R && path.size() == 4) {
            String str = "";
            for (int i = 0; i < 3; i++) {
                str += path.get(i);
                str += ".";
            }
            str += path.get(3);
            paths.add(str);
            return;
        }
        // 剪枝，序列没有分割完但是分割出来四个词
        if (path.size() == 4) {
            return;
        }
        // 按长度列举所有可能的分割方案，尝试分割出有效词的分割方案（十进制数不含前导0且范围在0~255区间）
        // 这里的取min也属于剪枝优化，不是必须的
        for (int i = 1; i <= Math.min(R - L + 1, 3); i++) {
            String str = s.substring(L, L + i);
            if (isValid(str)) {
                path.add(str);
                backtrace(s, L + i, R, path, paths);
                path.remove(path.size() - 1);
            }
        }
    }
    // 判断str是否是一个十进制数，且不含前导0
    private boolean isValid(String str) {
        if (str.length() > 1 && str.charAt(0) == '0') {
            return false;
        }
        int num = 0;
        for (int i = 0; i < str.length(); i++) {
            char c = str.charAt(i);
            if (c < '0' || c > '9') {
                return false;
            }
            num = num * 10 + c - '0';
        }
        return num >= 0 && num <= 255;
    }
}
```
