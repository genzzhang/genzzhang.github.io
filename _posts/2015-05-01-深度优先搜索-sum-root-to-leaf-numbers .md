---
layout: post
title: leetcode:【深度优先搜索】Unique Binary Search Trees 
date: 2015-05-01 14:01
categories: 算法
tags: leetcode dfs tree
---

# 问题描述 
```
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

    1
   / \
  2   3
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.
```

# code

```
/* 
从根节点遍历，若遍历到叶子结点，则sum+其路径的所有权值和 
当然，也可以把所有的遍历存储过来 
参考http://blog.csdn.net/csu54zzg/article/details/45039961 
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
public class Solution {  
    public int sumNumbers(TreeNode root) {  
        return dfs(root,0);  
    }  
    private int dfs(TreeNode root, int sum) {  
        //注意(1,#,2)  
        if(root == null) return 0;  
        sum = sum*10+root.val;  
        if(root.left == null && root.right == null)  
            return sum;  
        return dfs(root.left,sum)+dfs(root.right,sum);  
    }  
} 
```