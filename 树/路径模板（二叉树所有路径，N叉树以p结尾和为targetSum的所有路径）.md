## 二叉树的所有路径

dfs 求路径，每访问到一个结点就把它加到路径中，当来到叶子的时候收集路径结果。  

边界：空树时返回{}  

```java
class Solution {
    // O(n) O(n)
    public List<List<Integer>> binaryTreePaths(TreeNode root) {
        List<List<Integer>>  paths = new ArrayList<>();
        if (root == null) {
            return paths;
        }
        List<Integer> path = new ArrayList<>();
        backtrace(root, path, paths);
        return paths;
    }
    // 在以root为根的树中收集路径并将结果追加到path后，最终paths中存放的就是所有路径
    private void backtrace(TreeNode root, List<Integer> path, List<List<Integer>> paths) {
        path.add(root.val);
        if (root.left == null && root.right == null) {
            // 收集结果，将路径的拷贝加入结果集
            paths.add(new ArrayList<>(path));
            return;
        }
        if (root.left != null) {
            backtrace(root.left, path, paths);
            path.removeLast();
        }
        if (root.right != null) {
            backtrace(root.right, path, paths);
            path.removeLast();
        }
    }
}
```

## 扩展点

- 求 root 到 p 的路径：修改收集结果条件为 root == p
- 求树中和为 targetSum 的路径：扩展参数 curSum 和 targetSum， 遍历过程中不断累加 curSum ，当 curSum == targetSum相等时收集结果
- 多叉树路径问题：将二叉树枚举孩子的方式改为多叉树的扫描孩子列表
- 判断 XX 路径是否存在：将回溯方法返回值设为 boolean ，只要找到就返回

## 多叉树中以 p 结尾的和为 targetSum 的所有路径

测例：  
![](https://github.com/RossVermouth/algorithm/blob/main/%E9%99%84%E4%BB%B6/%E8%B7%AF%E5%BE%84%E5%92%8C.png)

```java
package leetcode.multipath;

import java.util.ArrayList;
import java.util.List;

public class Solution {
    // 求多叉树中以 p 结点结尾的和为 targetSum 的所有路径
    public List<List<Integer>> getPaths(Node root, Node p, int targetSum) {
        List<List<Integer>> paths = new ArrayList<>();
        if (root == null || p == null) {
            return paths;
        }
        List<Integer> path = new ArrayList<>();
        backtrace(root, p, 0, targetSum, path, paths);
        return paths;
    }
    // 从root开始找终点为p且累加和是targetSum的所有路径
    private void backtrace(Node root, Node p, int curSum, int targetSum,
                            List<Integer> path, List<List<Integer>> paths) {
        curSum += root.val;
        path.add(root.val);
        if (root == p && curSum == targetSum) {
            paths.add(new ArrayList<>(path));
            return;
        }
        for (Node child: root.children) {
            backtrace(child, p, curSum, targetSum, path, paths);
            path.remove(path.size() - 1);
        }
    }


    // dfs print tree for test
    private void printTree(Node root) {
        if (root == null) {
            return;
        }
        String res = root.val + " : ";
        for (Node child: root.children) {
            res += child.val + " ";
        }
        System.out.println(res);
        for (Node child: root.children) {
            printTree(child);
        }
    }

    public static void main(String[] args) {
        Node node1 = new Node(1, new ArrayList<>());
        Node node2 = new Node(2, new ArrayList<>());
        Node node3 = new Node(3, new ArrayList<>());
        Node node4 = new Node(4, new ArrayList<>());
        Node node5 = new Node(5, new ArrayList<>());
        Node node6 = new Node(6, new ArrayList<>());
        Node node7 = new Node(7, new ArrayList<>());
        Node node8 = new Node(8, new ArrayList<>());
        Node node9 = new Node(16, new ArrayList<>());
        Node node10 = new Node(13, new ArrayList<>());
        node1.children.add(node2);
        node1.children.add(node3);
        node2.children.add(node4);
        node2.children.add(node5);
        node2.children.add(node6);
        node3.children.add(node7);
        node3.children.add(node8);
        node5.children.add(node9);
        node7.children.add(node10);
        Solution solution = new Solution();
        System.out.println("树结构:");
        solution.printTree(node1);
        List<List<Integer>> res = solution.getPaths(node1, node9, 24);
        System.out.println("路径集:");
        for (int i = 0; i < res.size(); i++) {
            System.out.print("第" + i + "条路径:");
            for (int j = 0; j < res.get(i).size(); j++) {
                System.out.print(res.get(i).get(j) + " ");
            }
            System.out.println();
        }
    }
}

class Node {
    int val;
    List<Node> children;
    public Node() {}
    public Node(int val) {
        this.val = val;
    }
    public Node(int val, List<Node> children) {
        this.val = val;
        this.children = children;
    }
}
```
## 题目实例

[lc257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

```html
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

与模板的区别就是一条路径用 String 表示，基础类型变量下层递归的修改不会影响上层，故不需要显示回溯。

```java
class Solution {
    // O(n) O(n)
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> paths = new ArrayList<>();
        if (root == null) {
            return paths;
        }
        backtrace(root, "", paths);
        return paths;
    }
    // 在以root为根的树中收集路径并将结果追加到path后，最终paths中存放的就是所有路径
    private void backtrace(TreeNode root, String path, List<String> paths) {
        path += root.val;
        if (root.left == null && root.right == null) {
            paths.add(path);
            return;
        }
        path += "->";
        if (root.left != null) {
            backtrace(root.left, path, paths);
        }
        if (root.right != null) {
            backtrace(root.right, path, paths);
        }
    }
}
```

[lc112. 判断二叉树中是否存在和为targetSum的路径](https://leetcode.cn/problems/path-sum/)

```html
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true

输入：root = [], targetSum = 0
输出：false
```

判断而不用收集，精简参数不用记录路径，同时能够提前返回的就返回。

```java
class Solution {
    // O(n) O(n)
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return false;
        }
        return backtrace(root, 0, targetSum);
    }

    private boolean backtrace(TreeNode root, int curSum, int targetSum) {
        curSum += root.val;
        if (root.left == null && root.right == null && curSum == targetSum) {
            return true;
        }
        if (root.left != null) {
            if (backtrace(root.left, curSum, targetSum)) {
                return true;
            }
        }
        if (root.right != null) {
            return backtrace(root.right, curSum, targetSum);
        }
        return false;
    }
}
```

[lc113. 求二叉树中和为targetSum的路径](https://leetcode.cn/problems/path-sum-ii/)

```html
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]

输入：root = [1,2], targetSum = 0
输出：[]
```

```java
class Solution {
    // O(n) O(n)
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> paths = new ArrayList<>();
        if (root == null) {
            return paths;
        }
        List<Integer> path = new ArrayList<>();
        backtrace(root, 0, targetSum, path, paths);
        return paths;
    }
    // 从root开始以curSum作为基准开始搜索路径求累加和，如果路径总和等于targetSum，则将path收集
    private void backtrace(TreeNode root, int curSum, int targetSum, 
                            List<Integer> path, List<List<Integer>> paths) {
        curSum += root.val;
        path.add(root.val);
        if (root.left == null && root.right == null && curSum == targetSum) {
            paths.add(new ArrayList<>(path));
            return;
        }
        if (root.left != null) {
            backtrace(root.left, curSum, targetSum, path, paths);
            path.remove(path.size() - 1);
        }
        if (root.right != null) {
            backtrace(root.right, curSum, targetSum, path, paths);
            path.remove(path.size() - 1);
        }
    }
}
```

[129. 求根节点到叶节点数字之和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/)

```html
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
```

```java
class Solution {
    // 先求树的所有路径，再把所有路径转成数字求和即可 O(n) O(logn)/O(n) 
    public int sumNumbers(TreeNode root) {
        if (root == null) {
            return 0;
        }
        List<String> paths = new ArrayList<>();
        backtrace(root, "", paths);
        int sum = 0;
        for (String str: paths) {
            sum += Integer.valueOf(str);
        }
        return sum;
    }

    private void backtrace(TreeNode root, String path, List<String> paths) {
        path += root.val;
        if (root.left == null && root.right == null) {
            paths.add(path);
            return;
        }
        if (root.left != null) {
            backtrace(root.left, path, paths);
        }
        if (root.right != null) {
            backtrace(root.right, path, paths);
        }
    }
}
```







