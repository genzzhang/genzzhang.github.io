---
layout: post
title: leetcode:【动态规划】Longest Substring Without Repeating Characters
date: 2015-04-24 21:30
categories: 算法
tags: leetcode dp
---

# 问题描述

Given a string, find the length of the longest substring without repeating characters. For example, the longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3. For "bbbbb" the longest substring is "b", with the length of 1.

# code

```
/*
我现在能想到的就是dp了
dp[i]表示包含i索引位置的字符在内的Longest Substring Without Repeating Characters 
dp[i] = {dp[i-1]+1,i处字符未重复} OR {,i处字符在dp[i-1]之内有重复}
未重复，s.charAt(i)先前出现的位置不在(i-mark[s.charAt(i)],i-1)内
*/
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || s.isEmpty()) return 0;
        int len = s.length();
        if(len == 1) return 1;
        int[] mark = new int[256];
        int[] dp = new int[len];
        Arrays.fill(mark,-1);
        int max = 1;
        dp[0] = 1;
        mark[s.charAt(0)] = 0;
        for(int i = 1;i<len;i++) {
            if((mark[s.charAt(i)] + dp[i-1]) < i) {
                dp[i] = dp[i-1] +1;
            } else {
                dp[i] = i - mark[s.charAt(i)];
            }
            mark[s.charAt(i)] = i;
            max = Math.max(max,dp[i]);
        }
        return max;
    }
}


/*
google了一下
好像自己前面搞的有点复杂，但是思路还是dp的那个
以i为索引的字符结尾的最大非重复子字符串数目，end-start+1
因为没有必要记住所有的dp[i]，总是在Math.max中，记住前一个dp[i-1]就可以
所以开辟两个就行start和end，节省空间
*/
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s == null || s.isEmpty()) return 0;
        int len = s.length();
        if(len == 1) return 1;
        int[] mark = new int[256];
        int max = 1;
        int start = 0,end = 1;
        Arrays.fill(mark,-1);
        mark[s.charAt(0)] = 0;
        while(end < len) {
            int i = mark[s.charAt(end)];
            if(i >= start) start = i+1;
            mark[s.charAt(end)] = end;
            max = Math.max(max,end - start +1);
            end++;
        }
        return max;
    }
}

```