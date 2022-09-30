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

#### 题目4

[剑指 Offer II 053. 二叉搜索树中的中序后继](https://leetcode.cn/problems/P5rCT8/)  

给定一棵二叉搜索树和其中的一个节点 p ，找到该节点在树中的中序后继。如果节点没有中序后继，请返回 null 。  

节点 p 的后继是值比 p.val 大的节点中键值最小的节点，即按中序遍历的顺序节点 p 的下一个节点。  

```html
输入：root = [2,1,3], p = 1
输出：2
解释：这里 1 的中序后继是 2。请注意 p 和返回值都应是 TreeNode 类型。
```

先去左子树中找，找得到返回结果，找不到去 root 找， 找到返回结果，找不到去右子树找。

```java
class Solution {
    TreeNode prev = null;
    // O(l) (l是找得到p遍历结点数)  O(logn)/O(n)
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        if (root == null) {
            return null;
        }
        TreeNode res = inorderSuccessor(root.left, p);
        if (res != null) {
            return res;
        }
        if (prev == p) {
            return root;
        }
        prev = root;
        return inorderSuccessor(root.right, p);
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
否则说明要删除的就是 root，直接将左子树挂在右子树的最左侧结点左连接即可（或者将右子树挂在左子树的最右侧结点右链接即可）。

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
            // 将左子树挂在右子树最左侧结点的左链接下即可
            if (root.right == null) {
                return root.left;
            }
            TreeNode p = root.right;
            while (p.left != null) {
                p = p.left;
            }
            p.left = root.left;
            return root.right;
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

#### 题目7

[108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)  

使用有序数组构建高度平衡的二叉搜索树。返回一种可行结果。  

使用数组中点作为根，左侧序列递归构建左子树，右侧序列递归右子树即可。  

从BST角度而言：每次以中点作为根，其左侧值都小于中点值，中点值都小于右侧值正好符合BST的左子树值小于根节点值小于右子树值特性。  
从平衡树角度而言：每次都使左右子树的元素数目都最为相近，使用相同策略构建出的树高也最为相近（贪心）。

```java
class Solution {
    // O(n) O(logn)
    public TreeNode sortedArrayToBST(int[] nums) {
        if (nums == null || nums.length == 0) {
            return null;
        }
        return sortedArrayToBST(nums, 0, nums.length - 1);
    }

    private TreeNode sortedArrayToBST(int[] nums, int L, int R) {
        if (L > R) {
            return null;
        }
        if (L == R) {
            return new TreeNode(nums[L]);
        }
        int mid = L + ((R - L) >> 1);
        TreeNode root = new TreeNode(nums[mid]);
        root.left = sortedArrayToBST(nums, L, mid - 1);
        root.right = sortedArrayToBST(nums, mid + 1, R);
        return root;
    }
}
```

#### 题目8

[109. 将有序链表转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-list-to-binary-search-tree/)  

使用有序链表构建高度平衡的二叉搜索树。返回一种可行结果。  

- 解法一.推荐将链表转为数组再求解，时间 O(n) ， 空间 O(n) 
- 解法二.直接对链表操作，求链表中点并分割链表，时间 O(nlogn) ， 空间 O(logn)

```java
class Solution {
    // 每次使用链表中点元素创建树的根，使用前半部分创建左子树，右半部分创建右子树
    // 为了分割出左子树对应链表，实际找的是中点前驱 
    public TreeNode sortedListToBST(ListNode head) {
        if (head == null) {
            return null;
        }
        if (head.next == null) {
            return new TreeNode(head.val);
        }
        ListNode slow = head;
        ListNode fast = head.next.next;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        TreeNode root = new TreeNode(slow.next.val);
        ListNode rightHead = slow.next.next;
        slow.next = null;
        root.left = sortedListToBST(head);
        root.right = sortedListToBST(rightHead);
        return root;
    }
}
```

#### 题目9

[1382. 将二叉搜索树变平衡](https://leetcode.cn/problems/balance-a-binary-search-tree/)  

```java
class Solution {
    // 将BST先中序遍历得有序序列，再贪心构造平衡树 O(n) O(n)
    public TreeNode balanceBST(TreeNode root) {
        if (root == null) {
            return null;
        }
        List<Integer> list = new ArrayList<>();
        inOrder(root, list);
        int[] nums = new int[list.size()];
        for (int i = 0; i < nums.length; i++) {
            nums[i] = list.get(i);
        }
        return buildBalanceTree(nums, 0, nums.length - 1);
    }

    private void inOrder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        }
        inOrder(root.left, list);
        list.add(root.val);
        inOrder(root.right, list);
    }

    private TreeNode buildBalanceTree(int[] nums, int L, int R) {
        if (L > R) {
            return null;
        }
        if (L == R) {
            return new TreeNode(nums[L]);
        }
        int mid = L + ((R - L) >> 1);
        TreeNode root = new TreeNode(nums[mid]);
        root.left = buildBalanceTree(nums, L, mid - 1);
        root.right = buildBalanceTree(nums, mid + 1, R);
        return root;
    }
}
```













