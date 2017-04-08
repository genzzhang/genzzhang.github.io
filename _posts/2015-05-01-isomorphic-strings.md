---
layout: post
title: leetcode:Isomorphic Strings
date: 2015-05-01 21:52
categories: 算法
tags: leetcode string
---

# 问题描述 
```
Given two strings s and t, determine if they are isomorphic.

Two strings are isomorphic if the characters in s can be replaced to get t.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

For example,
Given "egg", "add", return true.

Given "foo", "bar", return false.

Given "paper", "title", return true.

Note:
You may assume both s and t have the same length.
```

# code

```
/*
自己感觉比较简单，也不知道我的这个方法会不会很绕
反正，我是觉得有点绕，其中由于int[]初始化全部为0，
所以，就从1开始计数位置
foozhz
its['f'] =1,its['o'] =2,its['o'] =2,its['z'] =3,,its['h'] =4,its['z'] =3
map(1,(1)) map(2,(2,3)) map(3,(4,6)),map(4,5)
*/
public class Solution {
    public boolean isIsomorphic(String s, String t) {
        Map<Integer,List<Integer>> map = new HashMap<Integer,List<Integer>>();
        int[] its = new int[256];
        int[] itt = new int[256];
        boolean flag = true;
        for(int i = 1;i<= s.length();i++) {
        	if(its[s.charAt(i-1)] == 0) {
        		its[s.charAt(i-1)] = i;
        		List<Integer> list = new ArrayList<Integer>();
        		list.add(i);
        		map.put(i, list);
        	} else {
        		map.get(its[s.charAt(i-1)]).add(i);
        	}
        	if(itt[t.charAt(i-1)] == 0)
        		itt[t.charAt(i-1)] = i;        	
        }
        for(int i = 1;i<=t.length();i++) {
            //两种情况：ab和ac || ab和aa 
        	if(map.get(itt[t.charAt(i-1)]) == null
        			|| !map.get(itt[t.charAt(i-1)]).contains(i)) {
        		flag = false;
        		break;
        	}
        }
        return flag;
    }
}

//后来一想，完全想复杂了，边判断边进行
public class Solution {
    public boolean isIsomorphic(String s, String t) {
        int[] its = new int[256];
        int[] itt = new int[256];
        boolean flag = true;
        for(int i = 1;i<=s.length();i++) {
            if(its[s.charAt(i-1)] ==0) {
                if(itt[t.charAt(i-1)] ==0) {
                    its[s.charAt(i-1)] = i;
                    itt[t.charAt(i-1)] = i;
                } else {
                    flag = false;
                    break;
                }
            } else {
                if(its[s.charAt(i-1)] != itt[t.charAt(i-1)]) {
                    flag = false;
                    break;
                } 
            }
        }
        return flag;
    }
}


```