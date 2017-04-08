---
layout: post
title: leetcode:Add Binary 
date: 2015-04-13 14:44
categories: 算法
tags: leetcode
---

#问题描述 

Given two binary strings, return their sum (also a binary string).

For example,  
a = "11"  
b = "1"  
Return "100"

#code

```
/*
注意
多一位
111+1=1000
110+1=【0111】=111
0+0=0
*/
public class Solution {
    public String addBinary(String a, String b) {
        char[] chara = a.toCharArray();
        char[] charb = b.toCharArray();
        int lena = chara.length;
        int lenb = charb.length;
        StringBuilder str = new StringBuilder();
        char carry = '0';
        int i = lena-1;
        int j = lenb-1;
        char[] ch = new char[2];
        while(lena >= 0 || lenb >= 0) {
        	lena--;
        	lenb--;
        	char a1,b1;
        	if(i<0) a1 = '0';
        	else {
        		a1 = chara[i];
        	}
        	if(j<0) b1 = '0';
        	else {
        		b1 = charb[j];
        	}
        	ch = getSumBinary(a1, b1, carry);
        	str.insert(0, ch[0]);
        	carry = ch[1];  
        	i--;
        	j--;
        }
        String out = str.toString().replaceAll("^(0)+", "");
        if(out.equals("")) out = "0";
        return out;
    }
    
    public char[] getSumBinary(char a,char b,char c) {
    	int value = a+b+c;
    	char[] ch = new char[2];
    	switch(value) {
        	case '0'+'0'+'0': ch[0]='0';ch[1]='0';break;
        	case '0'+'0'+'1': ch[0]='1';ch[1]='0';break;
        	case '0'+'1'+'1': ch[0]='0';ch[1]='1';break;
        	case '1'+'1'+'1': ch[0]='1';ch[1]='1';break;
    	}  
    	return ch;
    }
}
```