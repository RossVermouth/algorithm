## 题目链接

[leetcode 23](https://leetcode.cn/problems/merge-k-sorted-lists/submissions/)

## 题目描述

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

```html
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6

输入：lists = []
输出：[]

输入：lists = [[]]
输出：[]
```

## 解题思路

#### 方法一.迭代法

基于合并两个有序链表，滚动合并这 k 个有序链表。  

即扫描整个 lists， 扫描到第 i 条链表时， 将 lists[i] 与 lists[0] ~ lists[i - 1] 的合并结果合并，扫描完整个数组即得所有链表合并结果。  

时间复杂度 O(k^2 * n)， 空间复杂度 O(1)。

```java
class Solution {
    // O(k^2 * n) O(1), n 是链表平均长度
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        ListNode res = lists[0];
        for (int i = 1; i < lists.length; i++) {
            res = mergeTwoLists(res, lists[i]);
        }
        return res;
    }

    // O(m + n) O(1), m、n是两链表长
    private ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) {
            return list2;
        }
        if (list2 == null) {
            return list1;
        }
        ListNode dummy = new ListNode();
        ListNode tail = dummy;
        ListNode p = list1, q = list2;
        while (p != null && q != null) {
            if (p.val < q.val) {
                tail.next = p;
                p = p.next;
            } else {
                tail.next = q;
                q = q.next;
            }
            tail = tail.next;
            tail.next = null;
        }
        tail.next = p != null ? p : q;
        return dummy.next;
    }
}
```
#### 方法二.递归法

链表数组为空时，直接返回null。  

在 [L, R] 范围上进行链表合并等价于将 [L, mid] 区间合并结果 leftRes 与 [mid + 1, R] 区间合并结果 rightRes 进行合并。 

时间复杂度：  
考虑递归树，最下层有 k 个节点，故这一层有 k / 2 组合并且每一组合并需要 2n 代价，即最下层合并代价为 kn。  
倒数第二层有 k / 2 个节点， 故这一层有 k / 4组合并且每一组合并需要 4n 代价，即倒数第二层合并代价为 kn。
...  
叶子有 k 个节点得完全二叉树树高为 logk， 所有总的代价为 O(knlogk)。  

空间复杂度：递归栈即递归树高，O(logk)。

```java
class Solution {
    // O(knlogk) O(logk)
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists.length == 0) {
            return null;
        }
        return mergeKLists(lists, 0, lists.length - 1);
    }

    private ListNode mergeKLists(ListNode[] lists, int L, int R) {
        if (L > R) {
            return null;
        }
        if (L == R) {
            return lists[L];
        }
        int mid = L + ((R - L) >> 1);
        ListNode leftRes = mergeKLists(lists, L, mid);
        ListNode rightRes = mergeKLists(lists, mid + 1, R);
        return mergeTwoLists(leftRes, rightRes);
    }
    
    // O(m + n) O(1)
    private ListNode mergeTwoLists(ListNode list1, ListNode list2) {
        if (list1 == null) {
            return list2;
        }
        if (list2 == null) {
            return list1;
        }
        ListNode dummy = new ListNode();
        ListNode tail = dummy;
        ListNode p = list1, q = list2;
        while (p != null && q != null) {
            if (p.val < q.val) {
                tail.next = p;
                p = p.next;
            } else {
                tail.next = q;
                q = q.next;
            }
            tail = tail.next;
            tail.next = null;
        }
        tail.next = p != null ? p : q;
        return dummy.next;
    }
}
```


