## 题目1

[二叉树最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)  

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

[二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)  

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

[二叉树的结点数](https://leetcode.cn/problems/count-complete-tree-nodes/)  

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

[左叶子之和](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)  

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

[判断一棵树是否平衡](https://leetcode.cn/problems/balanced-binary-tree/)  



```java
class Solution {
    // 就是求最后一层的第一个结点 O(n) O(n)
    public int findBottomLeftValue(TreeNode root) {
        // 树必不为空
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int res = root.val;
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            TreeNode curNode = queue.poll();
            cur--;
            if (curNode.left != null) {
                queue.offer(curNode.left);
                next++;
            }
            if (curNode.right != null) {
                queue.offer(curNode.right);
                next++;
            }
            if (cur == 0 && !queue.isEmpty()) {
                res = queue.peek().val;
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}
```

## 题目6

[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)  
[117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E5%A1%AB%E5%85%85%E6%A0%91%E5%90%8E%E7%BB%A7.png)

要求将每一层从左到右串成链表, 116 是满二叉树，117 是普通二叉树，可使用以下 bfs 通用解。  

```java
class Solution {
    // 首选边遍历边处理，将每一层非最后结点的后继设为queue.peek
    // 换层时处理下一层结点，将下一层结点串成链表，这里边为了取结点，必须将queue设为List类型
    public Node connect(Node root) {
        if (root == null) {
            return root;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            Node curNode = queue.poll();;
            cur--;
            // 当前层还有结点即当前处理结点不是一层的尾结点
            if (cur > 0) {
                curNode.next = queue.peek();
            }
            if (curNode.left != null) {
                queue.offer(curNode.left);
                next++;
            }
            if (curNode.right != null) {
                queue.offer(curNode.right);
                next++;
            }
            if (cur == 0) {
                cur = next;
                next = 0;
            }
        }
        return root;
    }
}
```

附满二叉树的递归解法

```java
class Solution {
    // O(n) O(n)
    public Node connect(Node root) {
        if (root == null) {
            return root;
        }
        // 非叶子
        if (root.left != null) {
            // 因为满二叉树，左孩子存在右孩子必存在，左孩子后继就是右孩子
            root.left.next = root.right;
            // 右孩子的后继就是当前结点后继的左孩子（如果存在）
            root.right.next = root.next != null ? root.next.left : null;
            connect(root.left);
            connect(root.right);
        }
        return root;
    }
}
```

## 题目7

[二叉树的S形遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)  

```java
class Solution {
    // 偶数层反转
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        List<Integer> curLevel = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int cur = 1;
        int next = 0;
        boolean reverse = false;
        while (!queue.isEmpty()) {
            TreeNode curNode = queue.poll();
            curLevel.add(curNode.val);
            cur--;
            if (curNode.left != null) {
                queue.offer(curNode.left);
                next++;
            }
            if (curNode.right != null) {
                queue.offer(curNode.right);
                next++;
            }
            if (cur == 0) {
                if (reverse) {
                    Collections.reverse(curLevel);
                }
                res.add(curLevel);
                curLevel = new ArrayList<>();
                cur = next;
                next = 0;
                reverse = !reverse;
            }
        }
        return res;
    }
}
```
```java
class Solution {
    // 双栈法, 奇数层时从左向右入孩子到另一个栈才能实现从右向左出，偶数层从右向左入孩子...
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        List<Integer> curLevel = new LinkedList<>();
        Deque<TreeNode> curStack = new LinkedList<>();
        Deque<TreeNode> otherStack = new LinkedList<>();
        curStack.push(root);
        boolean odd = true;
        while (!curStack.isEmpty()) {
            TreeNode curNode = curStack.pop();
            curLevel.add(curNode.val);
            if (odd) {
                if (curNode.left != null) {
                    otherStack.push(curNode.left);
                }
                if (curNode.right != null) {
                    otherStack.push(curNode.right);
                }
            } else {
                if (curNode.right != null) {
                    otherStack.push(curNode.right);
                }
                if (curNode.left != null) {
                    otherStack.push(curNode.left);
                }
            }
            if (curStack.isEmpty()) {
                res.add(curLevel);
                curLevel = new LinkedList<>();
                Deque<TreeNode> temp = curStack;
                curStack = otherStack;
                otherStack = temp;
                odd = !odd;
            }
        }
        return res;
    }
}
```

## 题目8

[**微软真题**：对二叉树的每一层进行排序](https://www.nowcoder.com/discuss/954988)  

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E5%BE%AE%E8%BD%AF%E5%B1%82%E6%AC%A1%E9%81%8D%E5%8E%86.png)

从图中可以看出，在保持树结构不变的情况下，将每一层结点的值升序排序。  

使用二叉树层次遍历，在每次换层的时候取出结点的值，升序排序再进行值覆盖即可。  

```html
输入：
100 : 7 9 6 
7 : 33 10 
33 : 
10 : 
9 : 5 
5 : 
6 : 22 8 1 
22 : 
8 : 
1 : 
输出：
100 : 6 7 9 
6 : 1 5 
1 : 
5 : 
7 : 8 
8 : 
9 : 10 22 33 
10 : 
22 : 
33 : 
```

```java
package leetcode;

import java.util.*;

class TreeNode {
    int val;
    List<TreeNode> children;
    public TreeNode() {}
    public TreeNode(int val) {
        this.val = val;
    }
    public TreeNode(int val, List<TreeNode> children) {
        this.val = val;
        this.children = children;
    }
}

public class BfsSolution {
    // bfs
    public void sortLevel(TreeNode root) {
        if (root == null) {
            return;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            TreeNode curNode = queue.poll();
            cur--;
            for (TreeNode child: curNode.children) {
                queue.offer(child);
                next++;
            }
            if (cur == 0) {
                List<Integer> list = new ArrayList<>();
                for (TreeNode node: queue) {
                    list.add(node.val);
                }
                Collections.sort(list);
                int idx = 0;
                for (TreeNode node: queue) {
                    node.val = list.get(idx++);
                }
                cur = next;
                next = 0;
            }
        }
    }

    // dfs
    private void printTree(TreeNode root) {
        if (root == null) {
            return;
        }
        String res = root.val + " : ";
        for (TreeNode child: root.children) {
            res += child.val + " ";
        }
        System.out.println(res);
        for (TreeNode child: root.children) {
            printTree(child);
        }
    }

    public static void main(String[] args) {
        // test case 1: 题目测例
        TreeNode node1 = new TreeNode(100, new ArrayList<>());
        TreeNode node2 = new TreeNode(7, new ArrayList<>());
        TreeNode node3 = new TreeNode(9, new ArrayList<>());
        TreeNode node4 = new TreeNode(6, new ArrayList<>());
        TreeNode node5 = new TreeNode(33, new ArrayList<>());
        TreeNode node6 = new TreeNode(10, new ArrayList<>());
        TreeNode node7 = new TreeNode(5, new ArrayList<>());
        TreeNode node8 = new TreeNode(22, new ArrayList<>());
        TreeNode node9 = new TreeNode(8, new ArrayList<>());
        TreeNode node10 = new TreeNode(1, new ArrayList<>());
        node1.children.add(node2);
        node1.children.add(node3);
        node1.children.add(node4);
        node2.children.add(node5);
        node2.children.add(node6);
        node3.children.add(node7);
        node4.children.add(node8);
        node4.children.add(node9);
        node4.children.add(node10);
        BfsSolution bfsSolution = new BfsSolution();
        bfsSolution.sortLevel(node1);
        bfsSolution.printTree(node1);
        // test case2: 空树
        // test case3: 只有一个结点
    }
}
```
