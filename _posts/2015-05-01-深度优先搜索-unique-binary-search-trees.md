---
layout: post
title: leetcode:【深度优先搜索】Unique Binary Search Trees 
date: 2015-05-01 10:54
categories: 算法
tags: leetcode dfs tree
---

# 问题描述 
```
Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

# code

```
/*
参考http://blog.csdn.net/csu54zzg/article/details/45148007
以i为根节点的树，其左子树由[1, i-1]构成， 其右子树由[i+1, n]构成。
i=1,2,3..n，左右两边各有几个
f(0) = 1, f(1) = 1 
f(n) = f(0)*f(n-1)+f(1)*f(n-2)+...f(n-1)*f(0)
*/
public class Solution {
    public int numTrees(int n) {
        if(n<2) return 1;
        int[] f = new int[n+1];
        f[0] = f[1] = 1;
        for(int i = 2;i<n+1;i++) {
            f[i] = 0;
            for(int j = 0;j<i;j++) {
                f[i] += f[j]*f[i-1-j]; 
            }
        }
        return f[n];
    }
}
```


# 问题描述 

```
Given n, generate all structurally unique BST's (binary search trees) that store values 1...n.

For example,
Given n = 3, your program should return all 5 unique BST's shown below.

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3

```

# code

```
/*
以i为根节点的树，其左子树由[1, i-1]构成， 其右子树由[i+1, n]构成。
每一子树返回可能的各种组合根节点
递归, 对于[start, end]范围内的每个节点, 产生所有可能的左、右子树, 
再产生(#左子树 x #右子树)棵树, 返回所有的root nodes。
gen函数返回一个list of root nodes, 
每个root node所表示的树是由[start, end]这个范围内的数构成的BST。
*/
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; left = null; right = null; }
 * }
 */
public class Solution {
    public List<TreeNode> generateTrees(int n) {
        return generateTrees(1,n);
    }
    private List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        if(start > end) {
            list.add(null);
            return list;
        }
        for(int i = start;i<=end;i++) {
            List<TreeNode> treeLeft = new ArrayList<TreeNode>();
            List<TreeNode> treeRight = new ArrayList<TreeNode>();
            treeLeft = generateTrees(start, i-1);
            treeRight = generateTrees(i+1, end);
            for(int j = 0;j<treeLeft.size();j++)
                for(int k = 0;k<treeRight.size();k++) {
                    TreeNode root = new TreeNode(i);
                    root.left = treeLeft.get(j);
                    root.right = treeRight.get(k);
                    list.add(root);
                }
        }
        return list;
    }
}
```