## 题目链接

[leetcode 25](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

## 题目描述

给你链表的头节点 head ，每 k 个节点一组进行翻转，请你返回修改后的链表。

k 是一个正整数，它的值小于或等于链表的长度且大于等于 1。如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/2%E4%B8%AA%E4%B8%80%E7%BB%84%E5%8F%8D%E8%BD%AC.png)
```html
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```
![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/k%E4%B8%AA%E4%B8%80%E7%BB%84%E5%8F%8D%E8%BD%AC.png)
```html
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

## 解题思路

#### 解法一.迭代法

设四个指针 pre、start、end、next，分别表示当前处理段的前驱、当前处理段头节点、当前处理段尾节点、当前处理段的后继。 

记录表中第 k 个节点即为结果。

当 start 不空时，求end，end为空说明最后不足 k 个节点，直接返回结果，  

否则记录 next，提取当前段并进行反转，之后将前一段尾 prev 后继指向当前段头 end， 将当前段尾 start 后继指向下一段头 next， 

直到 start 为空，返回结果即可。

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/k%E4%B8%AA%E4%B8%80%E7%BB%84%E5%8F%8D%E8%BD%AC%E9%93%BE%E8%A1%A8.png)

```java
class Solution {
    // 迭代法 O(n) O(1) 要求head.length >= k >= 1
    public ListNode reverseKGroup(ListNode head, int k) {
        // 当前处理段的前驱
        ListNode prev = null;
        // 当前处理段的头
        ListNode start = head;
        // 当前处理段的尾
        ListNode end = null;
        // 当前处理段的后继
        ListNode next = null;
        // 必须放在初始化start的后边，原head后的第k个结点就是最终结果
        head = getKNodeFromStart(head, k);
        // 当存在处理段时
        while (start != null) {
            // 获取当前段的尾结点
            end = getKNodeFromStart(start, k);
            // 最后一段不足k个结点，直接返回head结果
            if (end == null) {
                return head;
            }
            // 记录next，并断开连接，便于翻转[start, end]区间
            next = end.next;
            end.next = null;
            reverse(start);
            // 将前一段的尾连上当前段的头
            if (prev != null) {
                prev.next = end;
            }
            // 将当前段的尾连上下一段的头
            start.next = next;
            // 后推指针，处理下一段
            prev = start;
            start = next;
        }
        return head;
    }

    private ListNode getKNodeFromStart(ListNode start, int k) {
        ListNode p = start;
        while (k > 1) {
            p = p.next;
            if (p == null) {
                break;
            }
            k--;
        }
        return p;
    }

    private ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode prev = null;
        ListNode cur = head;
        ListNode next = null;
        while (cur != null) {
            next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}
```

#### 解法二.递归法

当表长不足 k 时直接返回表头。

递归处理第k + 1个 ~ 第 n - 1 个节点部分，反转前 k 个节点，并调整两部分的引用关系。

返回原链表的第 k 个节点。

```JAVA
class Solution {
    // O(n) O(n) 要求head.length >= k >= 1
    public ListNode reverseKGroup(ListNode head, int k) {
        // 求表长
        int len = 0;
        ListNode p = head;
        while (p != null) {
            len++;
            p = p.next;
        }
        // 递归出口
        if (len < k) {
            return head;
        }
        // 找到第k个节点
        p = head;
        for (int i = 1; i <= k - 1; i++) {
            p = p.next;
        }
        // 递归处理第k个节点后边的部分
        ListNode othersRes = reverseKGroup(p.next, k);
        // 反转前k个节点
        p.next = null;
        ListNode res = reverse(head);
        // 建立链接
        head.next = othersRes;
        return res;
    }

    private ListNode reverse(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode res = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return res;
    }
}
```

