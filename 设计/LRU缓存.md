## 题目链接

[leetcode 146](https://leetcode.cn/problems/lru-cache/)

## 题目描述

请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。  
实现 LRUCache 类：  
LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存  
int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。  
void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该逐出最久未使用的关键字。  
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。  

```html
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

## 解题思路

考虑 key 在不在 map 中，将方法抽象：  

将key更新为最近使用 等价于 moveToTail  ...
将key更新为最少使用 等价于 moveToHead  ...

```JAVA
class LRUCache {
    private Map<Integer, ListNode> map;
    private ListNode head;
    private ListNode tail; 
    private int capacity;

    public LRUCache(int capacity) {
        map = new HashMap<>();
        head = new ListNode();
        tail = new ListNode();
        head.next = tail;
        tail.prev = head;
        this.capacity = capacity;
    }
    
    /**
     * key在map中，将key-value更新为最近使用，返回对应的value，不在则返回-1
     */
    public int get(int key) {
        ListNode exist = map.get(key);
        if (exist == null) {
            return -1;
        }
        moveToTail(exist);
        return exist.value;
    }

    /**
     * key在map中，将key-value更新为最近使用，并进行值覆盖
     * 否则，新建key-value插入到缓存中，如果缓存容量超过上限，还要将最近最少使用的元素移除
     */  
    public void put(int key, int value) {
        ListNode exist = map.get(key);
        if (exist == null) {
            ListNode node = new ListNode(key, value);
            map.put(key, node);
            addToTail(node);
            if (map.size() > capacity) {
                ListNode head = removeHead();
                map.remove(head.key);
            }       
        } else {
            moveToTail(exist);
            exist.value = value;
        }
    }

    private void moveToTail(ListNode node) {
        removeNode(node);
        addToTail(node);
    }

    private void addToTail(ListNode node) {
        node.next = tail;
        node.prev = tail.prev;
        node.next.prev = node;
        node.prev.next = node;
    }

    private ListNode removeHead() {
        ListNode res = head.next;
        removeNode(res);
        return res;
    }

    private void removeNode(ListNode node) {
        node.next.prev = node.prev;
        node.prev.next = node.next;
        node.next = null;
        node.prev = null;
    }
}

class ListNode {
    int key;
    int value;
    ListNode prev;
    ListNode next;
    public ListNode() {}
    public ListNode(int key, int value) {
        this.key = key;
        this.value = value;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

