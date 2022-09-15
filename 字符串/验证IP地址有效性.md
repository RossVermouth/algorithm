## 题目链接

[leetcode 468](https://leetcode.cn/problems/validate-ip-address/)

## 题目描述

给定一个字符串 queryIP。如果是有效的 IPv4 地址，返回 "IPv4" ；如果是有效的 IPv6 地址，返回 "IPv6" ；如果不是上述类型的 IP 地址，返回 "Neither" 。  

|  类型   | 段个数  |  段长度   | 段有效性  |
|  :-----  | :-----  |  :-----  | :-----  |
| IPv4  | 4 | 1-3  | 除0外不能含前导0，必须是一个数字即只能含0-9范围字符，数字范围在0-255范围内 |
| IPv6  | 8 | 1-4  | 必须是一个16进制数即只能含0-9，a-f，A-F范围字符（可以包含前导0） |

```html
// ipv4
// good case
192.0.0.1
// bad case1，段个数错误
192.0.0.1.
// bad case2，段长度错误
192..0.1
// bad case3，段格式错误之含前导0
192.01.0.1
// bad case4，段格式错误之含非法字符
192.1f.0.1
// bad case5，段格式错误之数字范围错误
192.256.0.1

// ipv6
// good case
2001:0db8:85a3:0000:0000:8a2e:0370:7334
// bad case1，段个数错误
2001:0db8:85a3:0000:0000:8a2e:0370:7334:
// bad case1，段长度错误
2001:0db8::00000:0000:8a2e:0370:7334
// bad case3，段格式错误之含非法字符
2001:0dbG:85a3:0.:0000:8a2e:0370:7334
```

## 解题思路

围绕**段个数，段长度，段格式**三点展开。  

```java
class Solution {
    // O(n) O(n)
    public String validIPAddress(String queryIP) {
        if (queryIP == null || queryIP.length() == 0) {
            return "Neither";
        }
        return isIPv4(queryIP) ? "IPv4" : (isIPv6(queryIP) ? "IPv6" : "Neither");
    }

    private boolean isIPv4(String queryIP) {
        // 1.分词并判断分词数组长度是否符合要求，分词时必须保留前后空格
        String[] strs = queryIP.split("\\.", -1);
        if (strs.length != 4) {
            return false;
        }
        // 2.对每个段进行校验
        for (int i = 0; i < 4; i++) {
            String str = strs[i];
            // 2.1 判断长度是否符合要求，特别地，ipv4 **段长度大于1**且含前导0时不满足要求
            if (str.length() == 0 || str.length() > 3 || (str.length() > 1 && str.charAt(0) == '0')) {
                return false;
            }
            // 2.2 将段转为数字，在转化的过程中遇到非数字字符则不满足要求，
            int num = 0;
            for (int j = 0; j < str.length(); j++) {
                char c = str.charAt(j);
                if (c < '0' || c > '9') {
                    return false;
                }
                num = num * 10 + c - '0';
            }
            // 2.3 对转化出的数字范围进行判断，如果不在0~255内，不满足要求
            if (num < 0 || num > 255) {
                return false;
            }
        }
        // 段个数为4，每个段长度都在1~3内，每个段除0外无前导0，每个段除数字外无其他字符，每个段对应的数字在0~255范围内
        return true;
    }

    private boolean isIPv6(String queryIP) {
        // 1.分词并判断分词数组长度是否符合要求，分词时必须保留前后空格
        String[] strs = queryIP.split("\\:", -1);
        if (strs.length != 8) {
            return false;
        }
        // 2.对每个段进行校验
        for (int i = 0; i < 8; i++) {
            String str = strs[i];
            // 2.1 判断长度是否符合要求
            if (str.length() == 0 || str.length() > 4) {
                return false;
            }
            // 2.2 判断每个段的字符是否仅包含0~9，a~f，A~F，使每个段都是16进制数
            for (int j = 0; j < str.length(); j++) {
                char c = str.charAt(j);
                if ((c < '0' || c > '9') && (c < 'a' || c > 'f') && (c < 'A' || c > 'F')) {
                    return false;
                }
            }
        }
        // 段个数为4，每个段长度都在1~4内，每个段都是16进制数(仅含0~9，a~f，A~F)
        return true;
    }
}
```
