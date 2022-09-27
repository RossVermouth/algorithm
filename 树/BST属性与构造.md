## 中序遍历类

#### 题目1

[530. 二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)  

给你一个二叉搜索树的根节点 root ，返回树中任意两不同节点值之间的最小差值。  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E6%9C%80%E5%B0%8F%E7%BB%9D%E5%AF%B9%E5%B7%AE.png)
```html
输入：root = [1,0,48,null,null,12,49]
输出：1
```

```java
class Solution {
    TreeNode prev = null;
    int min = Integer.MAX_VALUE;
    // 中序遍历 + 滚动前驱，BST的最小绝对差就是中序遍历序列min{相邻结点的差值绝对值}
    public int getMinimumDifference(TreeNode root) {
        if (root == null) {
            return 0;
        }
        getMinimumDifference(root.left);
        if (prev != null) {
            int delta = Math.abs(root.val - prev.val);
            min = Math.min(min, delta);
        }
        prev = root;
        getMinimumDifference(root.right);
        return min;
    }
}
```

#### 题目2

[501. 二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)  

给你一个含重复值的二叉搜索树（BST）的根节点 root ，找出并返回 BST 中的**所有**众数（即，出现频率最高的元素）。  

如果树中有不止一个众数，可以按**任意顺序**返回。

```html
输入：root = [1,null,2,2]
输出：[2]
```

```java
class Solution {
    public int[] findMode(TreeNode root) {
        inOrder(root);
        int[] res = new int[list.size()];
        for (int i = 0; i < res.length; i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    TreeNode prev = null; // 前驱
    int count = 0; // 当前值词频
    int max = 0; // 已扫描序列元素词频的最大值
    List<Integer> list = new ArrayList<>(); // 当前序列众数list
    // 中序遍历求众数，如果prev=null或prev.val != root.val count = 1 否则 count++；
    // 如果 count == max list添加当前值 如果 count > max list 添加当前值并且更新max
    private void inOrder(TreeNode root) {
        if (root == null) {
            return;
        }
        inOrder(root.left);
        if (prev == null || prev.val != root.val) {
            count = 1;
        } else { 
            count++; // bug3.值相同时要自增
        }
        // 是当前序列的众数，但不是唯一的众数
        if (count == max) {
            list.add(root.val);
        }
        // 是当前序列的众数，且是唯一众数
        if (count > max) {
            list.clear(); // bug2.唯一众数时忘clear之前元素
            list.add(root.val);
            max = count;
        }
        prev = root; // bug1.忘后推prev
        inOrder(root.right);
    }
}
```

## 二分搜索类

#### 题目1
[700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)  

给定二叉搜索树（BST）的根节点 root 和一个整数值 val。  

你需要在 BST 中找到节点值等于 val 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 null 。  

```java
class Solution {
    // 最基本的二分思想 O(logn)/O(n) O(logn)/O(n)
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        if (root.val < val) {
            return searchBST(root.right, val);
        }
        if (root.val > val) {
            return searchBST(root.left, val);
        }
        return root;
    }
}
```

