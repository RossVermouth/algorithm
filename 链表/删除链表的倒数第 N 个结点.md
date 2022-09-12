## 题目链接

[leetcode 19](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

## 题目描述

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。  

其中 n >= 1 && n <= len。 

```html
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]

输入：head = [1], n = 1
输出：[]
```

## 解题思路

为了将头节点一般化，设置哨兵 dummy。  

为了在一趟扫描中解决问题，设置快慢指针 slow ：= dummy，fast ：= dummy。  

根据题意链表中**必存在**要删除的节点，让 fast 先走 n 步， 之后当 fast 不为尾节点时，slow，fast同时后推。  

移除 slow 的后继节点，返回 **dummy.next** 即新的表头。

```java
class Solution {
    // 扫描一遍 O(n) O(1)
    // len >= 1, n >= 1 && n <= len
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode();
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = dummy;
        for (int k = 1; k <= n; k++) {
            fast = fast.next;
        }
        while (fast.next != null) {
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return dummy.next;
    }
}
```
