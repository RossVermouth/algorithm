## 题目1

[从下到上输出二叉树各层](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)  

```java
class Solution {
    // 层次遍历的最后将解逆置
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        List<Integer> curLevel = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int cur = 1;
        int next = 0;
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
                res.add(curLevel);
                curLevel = new ArrayList<>();
                cur = next;
                next = 0;
            }
        }
        Collections.reverse(res);
        return res;
    }
}
```

## 题目2

[二叉树右视图](https://leetcode.cn/problems/binary-tree-right-side-view/)  

```java
class Solution {
    // 层次遍历收集每层的最后一个结点即可
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
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
            if (cur == 0) {
                res.add(curNode.val);
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}
```

## 题目3

[二叉树的层平均值](https://leetcode.cn/problems/average-of-levels-in-binary-tree/)  

```java
class Solution {
    // method1：边遍历边求解 method2：换层统一求解，这里边 int -> Double 必须 (double)num 不能直接add
    public List<Double> averageOfLevels(TreeNode root) {
        List<Double> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        double sum = 0.0;
        int num = 1;
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            TreeNode curNode = queue.poll();
            sum += curNode.val;
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
                res.add(sum / num);
                sum = 0;
                num = next;
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}
```

## 题目4

[在每个树行中找最大值](https://leetcode.cn/problems/find-largest-value-in-each-tree-row/)  

```java
class Solution {
    // method1：边遍历边求解 method2：换层统一求解
    public List<Integer> largestValues(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        int max = Integer.MIN_VALUE;
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            TreeNode curNode = queue.poll();
            if (curNode.val > max) {
                max = curNode.val;
            }
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
                res.add(max);
                max = Integer.MIN_VALUE;
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}
```

## 题目5

[找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)  

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

[二叉树的最大宽度](https://leetcode.cn/problems/maximum-width-of-binary-tree/)  

给你一棵二叉树的根节点 root ，返回树的**最大宽度**。  

树的最大宽度是所有层中最大的宽度。  

每一层的宽度被定义为该层最左和最右的非空节点（即，两个端点）之间的长度。将这个二叉树视作与满二叉树结构相同，两端点间会出现一些延伸到这一层的 null 节点，**这些 null 节点也计入长度**。

![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E6%9C%80%E5%A4%A7%E5%AE%BD%E5%BA%A6.png)
```html
输入：root = [1,3,2,5,null,null,9,6,null,7]
输出：7
解释：最大宽度出现在树的第 4 层，宽度为 7 (6,null,null,null,null,null,7) 。  
```
将树的每个结点添加一个伴随 tag ，一个结点 node 其左右孩子的 tag 分别为 2tag + 1 和 2tag + 2。  

将树的每一层最右侧结点 tag 减最左侧结点 tag 再加 1 就是这一层的宽度，求出所有层的宽度取最大的即可。

```java
class Solution {
    // O(n) O(n)
    public int widthOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        Deque<Node> queue = new LinkedList<>();
        queue.addLast(new Node(root, 0));
        int res = 1;
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            Node curNode = queue.removeFirst();
            cur--;
            TreeNode curTreeNode = curNode.node;
            if (curTreeNode.left != null) {
                queue.addLast(new Node(curTreeNode.left, curNode.tag * 2 + 1));
                next++;
            }
            if (curTreeNode.right != null) {
                queue.addLast(new Node(curTreeNode.right, curNode.tag * 2 + 2));
                next++;
            }
            // 统一处理一层，必须对容器不空做判断
            if (cur == 0 && !queue.isEmpty()) {
                res = Math.max(res, queue.peekLast().tag - queue.peekFirst().tag + 1);
                cur = next;
                next = 0;
            }
        }
        return res;
    }
}

class Node {
    TreeNode node;
    int tag;
    public Node() {}
    public Node(TreeNode node, int tag) {
        this.node = node;
        this.tag = tag;
    }
}
```

## 题目9

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
