---
layout: post
title: leetcode:【动态规划】Longest Valid Parentheses   
date: 2015-04-21 00:23
categories: 算法
tags: leetcode dp
---

# 问题描述 Longest Valid Parentheses  

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4.

# code

```
/*
dp[i]表示从s[i]到s[s.length-1] 包含s[i]在内的的最长的有效匹配括号子串长度。
dp[s.length-1] = 0;
i从n-2 -> 0逆向求dp[]，并记录其最大值。
若s[i] == '('，则在s中从i开始到s.length-1计算dp[i]的值。( (()) ) ()
第一步查找与s[i]匹配的最大下标j = i + 1 + dp[i + 1]
注意dp[i + 1]已经在上一步求解，dp[i] =dp[i + 1] + 2
此外注意，在求得s[i ... j]的有效匹配长度之后，若j + 1没有越界，
则dp[i]的值还要加上从j+1开始的最长有效匹配，即dp[j + 1]。
*/
public class Solution {
    public int longestValidParentheses(String s) {
        int n = s.length();
        if(n < 2) return 0;
        int[] dp = new int[n];
        dp[n-1] = 0;
        int max = 0;
        for(int i = n-2;i>=0;i--) {
        	if(s.charAt(i) == ')') dp[i] = 0;
        	else {
        		int j = i+1+dp[i+1];
        		if(j < n && s.charAt(j) == ')') {
        		    dp[i] = dp[i+1] + 2;
        		    if(j+1 < n) dp[i] += dp[j+1];
        		}
        	}
        	max = Math.max(max, dp[i]);
        }
    	return max;
    }
}
```