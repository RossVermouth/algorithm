## 题目1

[lc104.二叉树最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)  

指的是树跟到最远叶子路径上的节点数，就是树高。

```java
class Solution {
    // 后序遍历，左右子树最大深度的较大者 + 1 O(n) O(n)
    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
    }
}
```

## 题目2

[lc111.二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)  

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

```html
输入：root = [3,9,20,null,null,15,7]
输出：2

输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

```java
class Solution {
    // 后序遍历，当有一侧孩子不存在时，用另一侧孩子的解+1，否则两侧解取较小值+1 O(n) O(n)
    public int minDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        if (root.left != null && root.right == null) {
            return minDepth(root.left) + 1;
        }
        if (root.left == null && root.right != null) {
            return minDepth(root.right) + 1;
        }
        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
}
```

## 题目3

[lc222.二叉树的结点数](https://leetcode.cn/problems/count-complete-tree-nodes/)  

```java
class Solution {
    // O(n) O(n)
    public int countNodes(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return countNodes(root.left) + countNodes(root.right) + 1;
    }
}
```

## 题目4

[lc404.左叶子之和](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)  

给定二叉树的根节点 root ，返回所有左叶子之和。  

```html
输入: root = [3,9,20,null,null,15,7] 
输出: 24 
解释: 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24

输入: root = [1]
输出: 0
```

```java
class Solution {
    // O(n) O(n) 处理当前结点（当前结点左孩子是左叶子则计和），递归处理左右子树
    public int sumOfLeftLeaves(TreeNode root) {
        if (root == null) {
            return 0;
        }
        // 必须是左孩子，并且必须是叶子才计数
        int sum = root.left != null && root.left.left == null && root.left.right == null ? root.left.val : 0;
        return sum + sumOfLeftLeaves(root.left) + sumOfLeftLeaves(root.right);
    }
}
```

## 题目5

[lc110.判断一棵树是否平衡](https://leetcode.cn/problems/balanced-binary-tree/)  

平衡二叉树指的是树要么为空，要么对于树中的任何一个结点其左右子树都是平衡二叉树且高度差的绝对值不超过 1 。

```java
class Solution {
    // 先序遍历 T(n) = 2 T(n/2) + O(n) 平均O(nlogn) O(logn) 最坏O(n^2) O(n)
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isBalanced(root.left) && isBalanced(root.right) && Math.abs(height(root.left) - height(root.right)) <= 1;
    }

    private int height(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return Math.max(height(root.left), height(root.right)) + 1;
    }
}
```

```java
class Solution {
    private static class Info {
        boolean isBalanced;
        int height;
        public Info() {}
        public Info(boolean isBalanced, int height) {
            this.isBalanced = isBalanced;
            this.height = height;
        }
    }
    // 基本树形dp 复杂度O(n) O(n)
    public boolean isBalanced(TreeNode root) {
        return getInfo(root).isBalanced;
    }

    private Info getInfo(TreeNode x) {
        if (x == null) {
            return new Info(true, 0);
        }
        Info leftInfo = getInfo(x.left);
        Info rightInfo = getInfo(x.right);
        boolean isBalanced = leftInfo.isBalanced && rightInfo.isBalanced && Math.abs(leftInfo.height - rightInfo.height) <= 1;
        int height = Math.max(leftInfo.height, rightInfo.height) + 1;
        return new Info(isBalanced, height);
    }
}
```

## 题目6

[lc98.判断一棵树是否是BST](https://leetcode.cn/problems/validate-binary-search-tree/)    

-2^31 <= Node.val <= 2^31 - 1  

BST指的是树要么为空，要么对于树中的任何一个结点其左右子树都是BST  
且左子树中最大值**小于**当前节点值，右子树中的最小值**大于**当前结点值。

```java
class Solution {
    private static class Info {
        boolean isBST;
        // 防溢出，规避 root(Integer.MAX_VALUE) 情况
        long max;
        long min;
        public Info() {}
        public Info(boolean isBST, long max, long min) {
            this.isBST = isBST;
            this.max = max;
            this.min = min;
        }
    }
    // 基本树形dp O(n) O(n)
    public boolean isValidBST(TreeNode root) {
        return getInfo(root).isBST;
    }

    private Info getInfo(TreeNode x) {
        if (x == null) {
            return new Info(true, Long.MIN_VALUE, Long.MAX_VALUE);
        }
        Info leftInfo = getInfo(x.left);
        Info rightInfo = getInfo(x.right);
        boolean isBST = leftInfo.isBST && rightInfo.isBST && leftInfo.max < x.val && rightInfo.min > x.val;
        long max = Math.max(Math.max(leftInfo.max, rightInfo.max), x.val);
        long min = Math.min(Math.min(leftInfo.min, rightInfo.min), x.val);
        return new Info(isBST, max, min);
    }
}
```

## 题目7

[lc226.翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E7%BF%BB%E8%BD%AC%E4%BA%8C%E5%8F%89%E6%A0%91.png) 
```html
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

```java
class Solution {
    // O(n) O(n)
    public TreeNode invertTree(TreeNode root) {
        if (root == null) {
            return root;
        }
        TreeNode temp = root.left;
        root.left = root.right;
        root.right = temp;
        invertTree(root.left);
        invertTree(root.right);
        return root;
    }
}
```

## 题目8

[lc101.对称二叉树](https://leetcode.cn/problems/symmetric-tree/)  

给你一个二叉树的根节点 root ， 检查它是否轴对称。  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%911.png)
```html
输入：root = [1,2,2,3,4,4,3]
输出：true
```

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E5%AF%B9%E7%A7%B0%E4%BA%8C%E5%8F%89%E6%A0%912.png)
```html
输入：root = [1,2,2,null,3,null,3]
输出：false
```

树镜像 <=> 根-左-右序列化结果和根-右-左序列化结果必须一致（必须带空指针）。

```java
class Solution {
    // O(n) O(n) 树镜像 <=> 根-左-右 w null == 根-右-左 w null
    public boolean isSymmetric(TreeNode root) {
        return isSymmetric(root, root);
    }

    private boolean isSymmetric(TreeNode root1, TreeNode root2) {
        if (root1 == null && root2 == null) {
            return true;
        }
        if (root1 != null && root2 == null) {
            return false;
        }
        if (root1 == null && root2 != null) {
            return false;
        }
        return root1.val == root2.val && isSymmetric(root1.left, root2.right) && isSymmetric(root1.right, root2.left);
    }
}
```

## 题目9

[lc617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)    

合并的规则是：从根开始，如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。  

返回合并后的二叉树。

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E5%90%88%E5%B9%B6%E4%BA%8C%E5%8F%89%E6%A0%91.png)
```html
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

```java
class Solution {
    // 一个树为空，结果就是另一个树，否则合并值并递归合并子树 O(min(n1, n2)) O(min(n1, n2))
    public TreeNode mergeTrees(TreeNode root1, TreeNode root2) {
        if (root1 == null) {
            return root2;
        }
        if (root2 == null) {
            return root1;
        }
        root1.val += root2.val;
        root1.left = mergeTrees(root1.left, root2.left);
        root1.right = mergeTrees(root1.right, root2.right);
        return root1;
    }
}
```
