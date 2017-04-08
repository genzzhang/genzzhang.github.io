---
layout: post
title: leetcode:【深度优先搜索】Generate Parentheses   
date: 2015-04-20 22:03
categories: 算法
tags: leetcode dfs
---

# 问题描述 Generate Parentheses    

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

"((()))", "(()())", "(())()", "()(())", "()()()"

# code

```
/*
深搜DFS的思路
递归的思路
*/
public class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> list = new ArrayList<String>();
        StringBuilder str = new StringBuilder();
        generate(list,str,0,0,2*n);
        return list;
    }
    private void generate(List<String> list,StringBuilder str,int left,int right,int n) {
        if(left + right == n) {
        	if(left == right) {
                list.add(str.toString());
        	}
            return;
        }
        if(left+right < n && left > right) {
        	if(str.length() < left+right+1)
        		str.append(')');
        	else {
            	str.setCharAt(left+right, ')');       		
        	}
            generate(list,str,left,right+1,n);
        }
        if(str.length() < left+right+1)
    		str.append('(');
    	else {
        	str.setCharAt(left+right, '(');       		
    	}
        generate(list,str,left+1,right,n);
    }
}
```