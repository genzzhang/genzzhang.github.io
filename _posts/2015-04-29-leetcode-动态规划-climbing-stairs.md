---
layout: post
title: leetcode:【动态规划】Climbing Stairs
date: 2015-04-29 20:46
categories: 算法
tags: leetcode dp
---

# 问题描述 
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

# code

```
public class Solution {
    public int climbStairs(int n) {
        //f(n) = f(n-1) + f(n-2),f(1) = 1,f(0) = 1;
        //表示当前位置所有不同走法数
        if(n < 2) return n;
        /*
        int[] f = new int[n+1];
        f[0] = 1;
        f[1] = 1;
        for(int i = 2;i<n+1;i++) {
            f[i] = f[i-1] + f[i-2];
        }
        */
        int a=1,b=1;
        for(int i = 2;i<n+1;i++) {
            b=a+b;
            a=b-a;
        }
        return b;
    }
}
```