---
layout: post
title: leetcode:Multiply Strings
date: 2015-04-10 23:24
categories: 算法
tags: leetcode
---

#问题描述

Given two numbers represented as strings, return multiplication of the numbers as a string.

Note: The numbers can be arbitrarily large and are non-negative.

#code

```
//              2   3   4  
//            × 5   6   7  
//------------------------  
//              14  21  28  
//          12  18  24  
//    + 10  15  20  
//------------------------  
//      10  27  52  45  28  
//------------------------  
//  1   3   2   6   7   8  
public class Solution {
    public static String multiply(String num1, String num2) {
        int lena = num1.length();
        StringBuilder num11 = new StringBuilder(num1).reverse();
        int lenb = num2.length();
        StringBuilder num22 = new StringBuilder(num2).reverse();
        int[] value = new int[lena+lenb];
        for(int i = 0;i<lena;i++)  {
            int t = num11.charAt(i)-'0';
            for(int j = 0;j<lenb;j++) {
                value[i+j] += t*(num22.charAt(j)-'0');   
            }
        }
        int carry = 0;
        StringBuilder str = new StringBuilder();
        for(int i = 0;i<lena+lenb;i++) {
            int sum = value[i]+carry;
            str.insert(0, sum%10);
            carry = sum/10;
        }
        String strend = str.toString();
        strend = strend.replaceAll("^(0+)", "");
        strend = strend.isEmpty()?"0":strend;
        return strend;
    }
}
```