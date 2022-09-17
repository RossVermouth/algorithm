## 题目链接

[leetcode 168](https://leetcode.cn/problems/excel-sheet-column-title/)

## 题目描述

实现将excel表列编号与表列名称互转的两个方法，表列编号与表列名称映射的关系：

```html
表列编号     表列名称
1            A
2            B
...
26           Z
27           AA
28           AB
...
52           AZ
```

## 解题思路

本质是 10 进制与 26 进制转换问题。  

将表列编号转为表列名称时，先将编号自减 1， 再进行转换。  

```java
class Solution {
    // O(logn) O(1)
    public String convertToTitle(int columnNumber) {
        StringBuilder sb = new StringBuilder();
        while (columnNumber != 0) {
            columnNumber--;
            // 必须下转
            sb.append((char)((columnNumber % 26) + 'A'));
            columnNumber /= 26;
        }
        return sb.reverse().toString();
    }
    
    // O(n) O(1)
    public int titleToNumber(String columnTitle) {
        int res = 0;
        for (int i = 0; i < columnTitle.length(); i++) {
            char c = columnTitle.charAt(i);
            res = res * 26 + c - 'A' + 1;
        }
        return res;
    }
}
```
