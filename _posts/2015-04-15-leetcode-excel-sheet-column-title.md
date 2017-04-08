---
layout: post
title: leetcode:Excel Sheet Column Title  
date: 2015-04-15 21:04
categories: 算法
tags: leetcode
---

# 问题描述
```
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
```

# code

```
//注意【0123456789】中的0
public class Solution {
    public String convertToTitle(int n) {
        StringBuilder str = new StringBuilder();
        int remainder=n;
        while(remainder>26) {
            if(remainder%26 == 0) {
                str.insert(0,(char)('Z'));
                remainder = remainder/26 -1;
            } else {
                str.insert(0,(char)(remainder%26-1+'A'));
                remainder/=26;
            }
        }
        if(remainder != 0)
            str.insert(0,(char)(remainder-1+'A'));
        return str.toString();
    }
}

进制转换，注意n的开始范围，对26取模的范围是0-25，每一步n--，每一步都是一个新的子数，目的是0-25对应A-Z。
public class Solution {
    public String convertToTitle(int n) {
        StringBuilder str = new StringBuilder();
        int remainder=n-1;
        while(remainder>=26) {
                str.insert(0,(char)(remainder%26+'A'));
                remainder = remainder/26 - 1;
        }
        str.insert(0,(char)(remainder+'A'));
        return str.toString();
    }
}
```