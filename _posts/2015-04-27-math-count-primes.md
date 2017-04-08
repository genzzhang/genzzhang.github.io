---
layout: post
title: leetcode:Count Primes  
date: 2015-04-27 12:50
categories: 算法
tags: leetcode math
---

# 问题描述 
Description:

Count the number of prime numbers less than a non-negative number, n

Hint: The number n could be in the order of 100,000 to 5,000,000.

click to show more hints.


# code

```
/*
自然数可分为1、素数和合数。
【厄拉多塞筛法】合数一定有一个不能再分解的因数
在2的上面画一个圆圈，然后划去2的其他倍数；
第一个既未画圈又没有被划去的数是3，将它画圈，再划去3的其他倍数；
现在既未画圈又没有被划去的第一个数 是5，将它画圈，并划去5的其他倍数……
依次类推，一直到所有小于或等于N的各数都画了圈或划去为止。
这时，画了圈的以及未划去的那些数正好就是小于 N的素数。
*/
public class Solution {
    public int countPrimes(int n) {
        //小于n的素数个数
        boolean[] mark = new boolean[n];
        for(int i = 3;i<n;i++)
            if(i%2 ==0) mark[i] = true;
        for(int i = 3;i<n;i = i+2) {
            for(int j = 2;j*i<n;j++)
                if(!mark[j])
                    mark[j*i] = true;
        }
        int count = 0;
        for(int i = 2;i<n;i++) 
            if(!mark[i]) count++;
        return count;
    }
}
```