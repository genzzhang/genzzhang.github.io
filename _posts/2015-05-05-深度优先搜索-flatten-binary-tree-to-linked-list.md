---
layout: post
title: leetcode:【深度优先算法】Flatten Binary Tree to Linked List  
date: 2015-05-05 10:11
categories: 算法
tags: leetcode tree dfs collection
---

# 问题描述   
```
Given a binary tree, flatten it to a linked list in-place.

For example,
Given

         1
        / \
       2   5
      / \   \
     3   4   6
The flattened tree should look like:
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```

# code

```
/*
不费脑力最粗暴的方法就是先序遍历list存起来
再将list数据处理下
*/
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void flatten(TreeNode root) {
        if(root == null) return;
        List<TreeNode> list = new ArrayList<TreeNode>();
        getList(list,root);
        for(int i = 0;i<list.size()-1;i++) {
            TreeNode cur = list.get(i);
            TreeNode next = list.get(i+1);
            cur.left = null;
            cur.right = next;
        }
        root = list.get(0);
    }
    private void getList(List<TreeNode> list,TreeNode root) {
        if(root == null) return;
        list.add(root);
        getList(list,root.left);
        getList(list,root.right);
    }
}



/*
tree中dfs也可以用stack来实现遍历，是个好想法
以前一直是bfs用queue是层遍历
*/
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void flatten(TreeNode root) {
        if(root == null) return;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        TreeNode lastNode = null;
        while(!stack.isEmpty()) {
            TreeNode temp = stack.pop();
            if(temp.right != null) stack.push(temp.right);
            if(temp.left != null) stack.push(temp.left);
            if(lastNode != null) {
                lastNode.left = null;
                lastNode.right = temp;
                lastNode = temp;
            } else {
                lastNode = temp;
            }
        }
    }
}
```