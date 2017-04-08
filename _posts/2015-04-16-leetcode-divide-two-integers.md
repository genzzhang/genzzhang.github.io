---
layout: post
title: leetcode:Divide Two Integers  
date: 2015-04-16 23:11 
categories: 算法
tags: leetcode
---

# 问题描述 Find Minimum in Rotated Sorted Array II 
 
Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

# code

```
//Submission Result: Time Limit Exceeded
public class Solution {
    public long divide(int dividend, int divisor) {
    	if(divisor == 0) return Integer.MAX_VALUE;
    	int dividendAbs = Math.abs(dividend);
        long sum = Math.abs(divisor), i = 0,getValue = 1;
        while(sum<=dividendAbs) {
            sum+=sum;
            i++;
        }
        if(i == 0) return 0;
        getValue<<=--i;
        sum>>=1;
        while(sum < dividendAbs) {
            getValue++;
        	sum+=divisor;
        }
        if(sum>dividendAbs) getValue--;
        return (dividend <= 0 && divisor >0 || dividend >= 0 && divisor <0) == true?-getValue:getValue;
    }
}
```