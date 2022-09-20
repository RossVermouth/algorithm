## 题目链接

[leetcode 347](https://leetcode.cn/problems/top-k-frequent-elements/)

## 题目描述

给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按任意顺序返回答案。

1 <= nums.length <= 105  
k 的取值范围是 [1, 数组中不相同的元素的个数]  
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的  

你所设计算法的时间复杂度 必须 优于 O(n log n) ，其中 n 是数组大小。

```html
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
```

## 解题思路

**topk 问题用堆，求前 k 大的用小顶堆。**  

首先，统计词频；  

之后建立小顶堆，小顶堆中存的是同时含有元素值和对应词频的 Map.Entry<Integer, Integer> ，小顶堆的容量初始为 k ，并且按词频升序建立比较器；  

扫描 map ，如果小顶堆元素数不足 k ，直接入当前 entry；否则需要比较堆顶和当前 entry 的词频，舍弃小的保留大的；  

最后，统计堆中剩余元素的值即为所求结果。  

注意：在 Java 中指定堆的大小只是初始容量，并不是容量上界。

```java
class Solution {
    // O(nlogk)  O(n) 
    public int[] topKFrequent(int[] nums, int k) {
        // 1.统计词频
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);
        }
        // 2.将val-count构成的entry放入小顶堆中，堆中元素数为k，并以count从小到大排序
        // 堆容量k只是初始容量，不是上界
        PriorityQueue<Map.Entry<Integer, Integer>> minPQ = new PriorityQueue<>(k, (e1, e2) -> {
            return e1.getValue() - e2.getValue();
        });
        // 手动保证堆中元素数目不超过k
        for (Map.Entry<Integer, Integer> entry: map.entrySet()) {
            if (minPQ.size() < k) {
                minPQ.offer(entry);
            } else if (minPQ.peek().getValue() < entry.getValue()) {
                minPQ.poll();
                minPQ.offer(entry);
            }
        }
        int[] res = new int[k];
        int idx = 0;
        while (!minPQ.isEmpty()) {
            res[idx++] = minPQ.poll().getKey();
        }
        return res;
    }
}
```

