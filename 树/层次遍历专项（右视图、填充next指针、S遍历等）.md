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

[116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)  
[117. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

要求将每一层从左到右串成链表, 116 是满二叉树，117 是普通二叉树，可使用以下 bfs 通用解。  

```java
class Solution {
    // 换层时处理下一层结点，将下一层结点串成链表，这里边为了取结点，必须将queue设为List类型
    public Node connect(Node root) {
        if (root == null) {
            return root;
        }
        List<Node> queue = new LinkedList<>();
        queue.add(root);
        int cur = 1;
        int next = 0;
        while (!queue.isEmpty()) {
            Node curNode = queue.remove(0);
            cur--;
            if (curNode.left != null) {
                queue.add(curNode.left);
                next++;
            }
            if (curNode.right != null) {
                queue.add(curNode.right);
                next++;
            }
            if (cur == 0 && !queue.isEmpty()) {
                for (int i = 0; i < queue.size() - 1; i++) {
                    Node node1 = queue.get(i);
                    Node node2 = queue.get(i + 1);
                    node1.next = node2;
                }
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

## 题目6

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
