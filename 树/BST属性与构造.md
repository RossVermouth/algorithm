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

#### 题目3

[538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)  

给出二叉搜索树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node 的新值等于原树中大于或等于 node.val 的值之和。  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E8%BD%AC%E7%B4%AF%E5%8A%A0%E6%A0%91.png)
```html
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

```java
class Solution {
    // 使用逆中序根右左遍历，每个节点赋值为当前节点值+前驱节点值即可
    // O(n) O(logn)/O(n)
    TreeNode prev = null;
    public TreeNode convertBST(TreeNode root) {
        if (root == null) {
            return null;
        }
        convertBST(root.right);
        if (prev != null) {
            root.val += prev.val;
        }
        prev = root;
        convertBST(root.left);
        return root;
    }
}
```

## 二分搜索类

#### 题目1
[700. 二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)  

给定二叉搜索树（BST）的根节点 root 和一个整数值 val。  

你需要在 BST 中找到节点值等于 val 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 null 。  

- 递归法
```java
class Solution {
    // 递归法，最基本的二分思想 O(logn)/O(n) O(logn)/O(n)
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
- 迭代法
```java
class Solution {
    // 最基本的二分思想 O(logn)/O(n) O(1)
    public TreeNode searchBST(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        TreeNode p = root;
        while (p != null) {
            if (p.val < val) {
                p = p.right;
            } else if (p .val > val) {
                p = p.left;
            } else {
                return p;
            }
        }
        return null;
    }
}
```

#### 题目2

[230. 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)  

给定一个二叉搜索树的根节点 root ，和一个整数 k ，请你设计一个算法查找其中第 k 个最小元素（从 1 开始计数）。  

树中的节点数为 n   
1 <= k <= n <= 104  
0 <= Node.val <= 104  

- 迭代法  
使用中序遍历搜索结点，查找到第 k 个就返回。

```java
class Solution {
    // 中序遍历找到第k个元素，递归必须有出口，null时返回一个占位元素意味没有找到结果
    // 去左子树寻找，找到结果返回，否则判断根是否是第k个元素，是的话返回，否则返回右子树结果
    int count = 0;
    public int kthSmallest(TreeNode root, int k) {
        if (root == null) {
            return -1;
        }
        int res = kthSmallest(root.left, k);
        if (res >= 0) {
            return res;
        }
        count++;
        if (count == k) {
            return root.val;
        }
        return kthSmallest(root.right, k);
    }
}
```

- 二分法  
在 root 为根的树中查找第 k 小的元素，先求左子树中的结点数 n :  
如果 n >= k ，去左子树找第 k 小元素；  
否则如果 n + 1 == k ， root.val 即为结果；  
否则去右子树中找第 n - k - 1 小的元素。  

```java
class Solution {
    // 二分法 T(n) = T(n / 2) + O(n) => 时间O(n) 空间O(logn)
    public int kthSmallest(TreeNode root, int k) {
        int n = getNodeCount(root.left);
        if (n >= k) {
            return kthSmallest(root.left, k);
        } else if (n + 1 == k) {
            return root.val;
        } else {
            return kthSmallest(root.right, k - n - 1);
        }
    }

    private int getNodeCount(TreeNode root) {
        if (root == null) {
            return 0;
        }
        return getNodeCount(root.left) + getNodeCount(root.right) + 1;
    }
}
```

#### 题目3

[701. 二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)  

给定 root ， val ， 将 val 插入到树中。   

所有值 Node.val 是 独一无二 的。  
保证 val 在原始BST中不存在。  

```java
class Solution {
    // 将val拆入到root为根的子树中并返回新树的根 O(logn)/O(n) O(logn)/O(n) 
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        if (root.val < val) {
            root.right = insertIntoBST(root.right, val);
        } else if (root.val > val) {
            root.left = insertIntoBST(root.left, val);
        }
        return root;
    }
}
```

#### 题目4

[450. 二叉搜索树中的删除操作](https://leetcode.cn/problems/delete-node-in-a-bst/)    

给定 root ， val ， 将 val 从树中删除。      

所有值 Node.val 是 独一无二 的。    
val 可能并不在树中存在。  

**解答：**  
如果树为空，则直接返回 null ；  
如果 val > root.val ， 去右子树中删除，并把新的右子树的根挂在 root.right 下，返回 root ；  
否则如果 val < root.val ， 去左子树中删除，并把新的左子树的根挂在 root.left 下，返回 root;  
否则说明要删除的就是 root 。  

在 BST 中删除 root 的具体算法为：  
如果 root 为空， 直接返回空；  
如果 root.left 为空， root.right 即为删除后的树的根；  
如果 root.right 为空， root.left 即为删除后的树的根；  
否则找到右子树中的最小结点，将其在右子树中删除，并将原树的左右子树挂在该结点的左右链接下，该结点即为新树的根。  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/bst%E5%88%A0%E9%99%A4.png)

```java
class Solution {
    // 比较难细节比较多，多加注意
    // O(logn)/O(n) O(logn)/O(n)
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return root;
        }
        if (root.val < key) {
            root.right = deleteNode(root.right, key);
        } else if (root.val > key) {
            root.left = deleteNode(root.left, key);
        } else {
            if (root.left == null) {
                return root.right;
            }
            if (root.right == null) {
                return root.left;
            }
            TreeNode p = root.right;
            if (p.left == null) {
                p.left = root.left;
                return p;
            }
            // 找到右子树中最左侧节点, 将其从右子树中删除
            while (p.left != null && p.left.left != null) {
                p = p.left;
            }
            TreeNode q = p.left;
            // 重大bug点,删除这个结点时,需要保留该节点的右子树
            p.left = q.right;
            // 将原root的左右子树挂在原右子树最左侧节点的左右连接下
            q.left = root.left;
            q.right = root.right;
            return q;
        }
        return root;
    }
}
```

#### 题目5

[669. 修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)  

给你BST root 和区间 [low， high] ，将树 trim 到这个区间并保留原结点的相对位置关系。  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/trimTree.png)  
```html
输入：root = [3,0,4,null,2,null,null,1], low = 1, high = 3
输出：[3,2,null,1]
```

```java
class Solution {
    // O(logn)/O(n) O(logn)/O(n)
    public TreeNode trimBST(TreeNode root, int low, int high) {
        if (root == null) {
            return null;
        }
        if (root.val < low) {
            return trimBST(root.right, low, high);
        }
        if (root.val > high) {
            return trimBST(root.left, low, high);
        }
        root.left = trimBST(root.left, low, high);
        root.right = trimBST(root.right, low, high);
        return root;
    }
}
```

#### 题目6

[235. 二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)  

在 BST 中找两节点 p 、 q 的最近公共最先。   

最近公共最先指的是所有祖先中深度最大的。   

如果 p q 都在右子树中，去右子树中找；  
如果 p q 都在左子树中，去左子树中找；  
否则 root 即为最近公祖。  

```java
class Solution {
    // O(logn)/O(n) O(1)（尾递归）
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if (p.val > root.val && q.val > root.val) {
            return lowestCommonAncestor(root.right, p, q);
        }
        if (p.val < root.val && q.val < root.val) {
            return lowestCommonAncestor(root.left, p, q);
        }
        return root;
    }
}
```







