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

要求将每一层从左到右串成链表。  

![](https://sukiui.com/i/2022/09/21/10hi7vk.png)


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

## 题目6

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

## 题目7

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



