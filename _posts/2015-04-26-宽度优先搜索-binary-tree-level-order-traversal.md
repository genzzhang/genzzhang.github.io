---
layout: post
title: leetcode:【宽度优先搜索】Binary Tree Level Order Traversal I && II
date: 2015-04-26 22:21
categories: 算法
tags: leetcode tree bfs
---

# 问题描述   Binary Tree Level Order Traversal  
```
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree {3,9,20,#,#,15,7},
    3
   / \
  9  20
    /  \
   15   7
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]
``

# code

```
/*
【BFS通常框架】
通常使用队列实现FIFO
    初始化队列Q
    Q={起点s}，s标记为已经访问
    while(Q不为空) {
        取队首head，head出队
        if(head处达到目标) {...}
        所有head未访问的相邻节点加入队列
        标记head已经被访问
    }
eg：
int[] queue = new int[n];
int head = 0;tail = 0;
queue[tail++] = root;
while(head < tail) {
   //do thing 
}
【三种思路，最后一种最为简洁】
===【每个节点记录自己所在的层数】
一个list记录遍历的先后结构，一个list记录遍历的节点的层数
如果输出遍历路线，一般还要保持父节点的遍历中存的位置
最后将两个list的结果对照来输出
===【用一个符号来比较层的转移】
用一个变量节点来记录下一层的开始节点
while(!Q.isEmpty()) {
    Node n = Q.peek();
    Node n2depth = null'
    while(n != n2depth && !Q.isEmpty()) {
        Q.remove();
        if(n2depth == null) {
            n2depth = n not null 子节点
        }
        Q.add(n not null 子节); n = Q.peek();
    }
    //do thing for this layer
}
===【用两个队列来存上下两层的数据】
从root开始，放到第一个队列
接着取出第一队列，将其相邻或子节点放到下一个队列
如此反复
*/
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 
 /*
Input:	{}
Output:	null
Expected:[]
 */
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> lists = new ArrayList<List<Integer>>();
        if(root == null) return lists;
        List<Integer> list; //注意每次都要new
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        while(!queue.isEmpty()) {
            Queue<TreeNode> temp = new LinkedList<TreeNode>();
            list = new ArrayList<Integer>();
            while(!queue.isEmpty()) {
                TreeNode n = queue.remove();
                list.add(n.val);
                if(n.left != null) temp.add(n.left);
                if(n.right != null) temp.add(n.right);
            }
            lists.add(list);
            queue = temp;
        }
        return lists;
    }
}
```

# 问题描述 Binary Tree Level Order Traversal II   
```
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree {3,9,20,#,#,15,7},
    3
   / \
  9  20
    /  \
   15   7
return its bottom-up level order traversal as:
[
  [15,7],
  [9,20],
  [3]
]
```

# code

```
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
 
 /*没啥好搞的
 Colletcions.reverse(lists);
 */
public class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> lists = new ArrayList<List<Integer>>();
        if(root == null) return lists;
        List<Integer> list; //注意每次都要new
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        while(!queue.isEmpty()) {
            Queue<TreeNode> temp = new LinkedList<TreeNode>();
            list = new ArrayList<Integer>();
            while(!queue.isEmpty()) {
                TreeNode n = queue.remove();
                list.add(n.val);
                if(n.left != null) temp.add(n.left);
                if(n.right != null) temp.add(n.right);
            }
            lists.add(list);
            queue = temp;
        }
        Collections.reverse(lists);
        return lists;
    }
}
```

