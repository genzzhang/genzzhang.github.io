---
layout: post
title: leetcode:【深度优先搜索】与Tree  
date: 2015-04-14 11:51
categories: 算法
tags: leetcode tree dfs
---

# 问题描述 Balanced Binary Tree   
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

# code

```
/*
 判断当前节点是否平衡，取决于左右子树的高度不相差1
 如果root节点平衡，则继续判断左右子树是否平衡
【自底向下】，其实是重复遍历了2次，一次是取高度，一次是判平衡
 */
public class Solution {
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;     
        int LeftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        if(Math.abs(LeftHeight-rightHeight)>1) return false;
        return (isBalanced(root.left)&&isBalanced(root.right));
    }
    
    private int getHeight(TreeNode root) {
        if(root == null) return 0;
        int LeftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        return 1+Math.max(LeftHeight,rightHeight);
    }
}



 /*
 自下而上
 子节点不平衡则返回高度-1
 判断当前节点是否平衡，取决于:
 左右子树都平衡且高度不相差1
 返回信息包含高度以及平衡否，若不平衡则返回高度-1
 只要有高度为-1则整个都肯定不平衡，全部返回-1
 判断root高度是否为-1即可
 */
public class Solution {
    public boolean isBalanced(TreeNode root) {
        return getHeight(root) != -1;
    }
    
    private int getHeight(TreeNode root) {
        if(root == null) return 0;
        int LeftHeight = getHeight(root.left);
        int rightHeight = getHeight(root.right);
        if(LeftHeight == -1 || rightHeight == -1 || Math.abs(LeftHeight-rightHeight)>1)
            return -1;
        return 1+Math.max(LeftHeight,rightHeight);
    }
}
```

# 问题描述 Same Tree

Given two binary trees, write a function to check if they are equal or not.  

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

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
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
        //两者均为空，视为相等
        if(p==null && q==null) return true;
        //任意一个为空或者不相等，视为不相等
        if(p==null && q!=null || p!=null && q==null || q.val != p.val) return false;
        //接着往下判断
        return(isSameTree(p.left,q.left) && isSameTree(q.right,p.right));
    }
}
```


# 问题描述 Symmetric Tree  
```
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree is symmetric:

    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following is not:
    1
   / \
  2   2
   \   \
   3    3
Note:
Bonus points if you could solve it both recursively and iteratively.

confused what "{1,#,2,3}" means? 
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
public class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        return isSymmetric(root.left,root.right);
    }
    private boolean isSymmetric(TreeNode left,TreeNode right) {
        if(left == null && right == null) return true;
        if((left ==null && right != null) || (left !=null && right == null) || (left.val != right.val)) return false;
        return isSymmetric(left.left,right.right) && isSymmetric(left.right,right.left);
    }    
}
```


# 问题描述 Path Sum
```
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
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
public class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        return pathSum(root,sum);
    }
    private boolean pathSum(TreeNode root,int sum) {
        if(root == null) return false;
        //判断是否为叶子节点且刚好找到sum
        if(root.val == sum && root.left == null&& root.right == null) return true;
        int value = sum - root.val;
        return pathSum(root.left,value) || pathSum(root.right,value);
    }
}
```


# 问题描述 Path Sum II  
```
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
return

[
   [5,4,11,2],
   [5,8,4,5]
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
/*
Path Sum I可以做，Path Sum II就比较简单
难度在于数据的存储
对于非固定长度的，用stack来存储比较合适
注意：反转和push、pop
*/
public class Solution {
    public List<ArrayList<Integer>> pathSum(TreeNode root, int sum) {
        List<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();
        getList(root,list,new Stack<Integer>(),sum);
        return list;
    }
    private void getList(TreeNode root,List<ArrayList<Integer>> list,Stack<Integer> stack,int sum) {
        if(root == null) return;
        Integer key = root.val;
        stack.push(key);
        if(root.left == null && root.right == null && root.val == sum) {
        	Stack<Integer> s = (Stack<Integer>)stack.clone();
            //特别note这个pop，push和pop永远是一对
        	stack.pop();
        	ArrayList<Integer> listItem = new ArrayList<Integer>();
        	while(!s.isEmpty()) {
        		listItem.add(s.pop());
        	}
        	//由于是栈，先进后出，则要重新排序，纠正回来
        	int len = listItem.size();
        	for(int i = 0;i<len/2;i++) 
        	    listItem.set(len-1-i, listItem.set(i, listItem.get(len-1-i)));
        	list.add(listItem);
        	return;
        }
        getList(root.left, list, stack, sum-key);
        getList(root.right, list, stack, sum-key);
        stack.pop();
        return ;       
    }
}


/*
自己想的的确比较烂
这个是个好方法
简洁明了
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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> ret = new LinkedList<List<Integer>>();
        dfs(root, sum, new LinkedList<Integer>(), ret);
        return ret;
    }
    private void dfs(TreeNode node, int sum, List<Integer> currentPath, List<List<Integer>> result) {
        if (node == null) {
            return;
        }
        currentPath.add(node.val);
        if (node.val == sum && node.left == null && node.right == null) {
            result.add(new LinkedList<Integer>(currentPath));
        }
        else {
            dfs(node.left, sum - node.val, currentPath, result);
            dfs(node.right, sum - node.val, currentPath, result);
        }
        currentPath.remove(currentPath.size() -1);
        return;
    }
}
```