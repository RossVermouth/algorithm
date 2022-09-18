## 题目链接

[leetcode 705](https://leetcode.cn/problems/design-hashset/)

## 题目描述

不使用任何内建的哈希表库设计一个哈希集合（HashSet）。

实现 MyHashSet 类：

void add(key) 向哈希集合中插入值 key 。  
bool contains(key) 返回哈希集合中是否存在这个值 key 。  
void remove(key) 将给定值 key 从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。   

最多调用 10^4 次 add、remove 和 contains

```html
输入：
["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
输出：
[null, null, null, true, false, null, true, null, false]

解释：
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // 返回 True
myHashSet.contains(3); // 返回 False ，（未找到）
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // 返回 True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // 返回 False ，（已移除）
```

## 解题思路

底层数据结构是链表数组，在 Java 中使用内外层均为 ArrayList 构建的 table 即可，并限制 table 的长度。  

实现方法的核心思路是**将元素映射到对应的桶，在对应的桶中执行相应的操作**。


```java
class MyHashSet {
    private List<List<Integer>> table;

    private static final int TABLE_LENGTH = 1024;

    public MyHashSet() {
        table = new ArrayList<>(TABLE_LENGTH);
        for (int i = 0; i < TABLE_LENGTH; i++) {
            table.add(new ArrayList<>());
        }
    }
    // 先取桶，再在桶内进行增删判断是否存在等操作，每个桶平均长度n / 1024，根据题意n不超过10000，故contains方法O(1)
    public void add(int key) {
        int idx = hash(key);
        List<Integer> bucket = table.get(idx);
        if (bucket.contains(key)) {
            return;
        }
        bucket.add(key);
    }
    
    public void remove(int key) {
        int idx = hash(key);
        List<Integer> bucket = table.get(idx);
        if (!bucket.contains(key)) {
            return;
        }
        bucket.remove(new Integer(key));
    }
    
    public boolean contains(int key) {
        int idx = hash(key);
        List<Integer> bucket = table.get(idx);
        return bucket.contains(key);
    }
    // 将key映射到对应的桶
    private int hash(int key) {
        return Math.abs(key) % TABLE_LENGTH;
    }
}
```
