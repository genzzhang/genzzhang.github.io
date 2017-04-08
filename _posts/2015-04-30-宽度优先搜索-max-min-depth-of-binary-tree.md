---
layout: post
title: leetcode:【宽度优先搜索】Maximum | Minimum Depth of Binary Tree
date: 2015-04-30 19:14
categories: 算法
tags: leetcode bfs
---

# 问题描述 Maximum Depth of Binary Tree  

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

# code

```
/*
 * 
 * 参考http://blog.csdn.net/csu54zzg/article/details/45293363
 * 
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null) return 0;
        Queue<TreeNode> queue  = new LinkedList<TreeNode>();
        queue.add(root);
        int depth = 0;
        while(!queue.isEmpty()) {
            TreeNode nodeNext = null;
            TreeNode node = queue.peek();
            depth++;
            while(!queue.isEmpty() && nodeNext != node) {
                node = queue.remove();
                if(nodeNext == null) {
                    if(node.left != null) nodeNext = node.left;
                    else if(node.right != null) nodeNext = node.right;
                }
                if(node.left != null)
                    queue.add(node.left);
                if(node.right != null)
                    queue.add(node.right);
                // returns null if this queue is empty.
                node = queue.peek();
            }
        }
        return depth;
    }
}
```


# 问题描述 Minimum Depth of Binary Tree    

Given a binary tree, find its Minimum depth.

# code

```
/**
 * 
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int minDepth(TreeNode root) {
        if(root == null) return 0;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        int depth = 0;
        queue.add(root);
        while(!queue.isEmpty()) {
            TreeNode node = queue.peek();
            TreeNode nodeNext = null;
            depth++;
            boolean flag = false;
            while(!queue.isEmpty() && node != nodeNext) {
                node = queue.remove();
                if(nodeNext == null) {  
                    if(node.left != null) nodeNext = node.left;  
                    else if(node.right != null) nodeNext = node.right;  
                }  
                if(node.left != null)  
                    queue.add(node.left);  
                if(node.right != null)  
                    queue.add(node.right);  
                if(node.left == null && node.right == null) {
                    flag = true;
                    break;
                }
                node = queue.peek();  
            }
            if(flag) break;
        }
        return depth;
    }
}
```