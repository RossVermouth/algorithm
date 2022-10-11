## 题目链接

[leetcode 6](https://leetcode.cn/problems/zigzag-conversion/)

## 题目描述

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。  

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：  

```html
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。    

请你实现这个将字符串进行指定行数变换的函数。  

1 <= numRows <= 1000  

```html
输入：s = "A", numRows = 1
输出："A"

输入：s = "PAYPALISHIRING", numRows = 4
输出："PINALSIGYAHRPI"
解释：
P     I    N
A   L S  I G
Y A   H R
P     I
```

## 解题思路

首先使用二维表存储中间的Z字形排列，表中每一行使用 StringBuilder 即可。  

扫描字符串中的字符，依次加入第 0 ， 1 ， ... ， numRows - 1 ， numRows - 2， ... ， 1， 0， ... 行。  

行优先将每行字符串顺序拼接得最终结果。  

trick： 可以使用 flag 改变行标变化方向 (至少两行时适用)。

```JAVA
class Solution {
    // O(n) O(n)
    public String convert(String s, int numRows) {
        if (numRows == 1) {
            return s;
        }
        List<StringBuilder> table = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {
            table.add(new StringBuilder());
        }
        int i = 0, flag = -1;
        for (char c: s.toCharArray()) {
            table.get(i).append(c);
            if (i == 0 || i == numRows - 1) {
                flag = -flag;
            }
            i += flag;
        }
        StringBuilder res = new StringBuilder();
        for (StringBuilder sb: table) {
            res.append(sb);
        }
        return res.toString();
    }
}
```

