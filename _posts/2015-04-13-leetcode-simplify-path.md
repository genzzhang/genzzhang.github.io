---
layout: post
title: leetcode:Simplify Path
date: 2015-04-13 13:06
categories: 算法
tags: leetcode
---

#问题描述
Given an absolute path for a file (Unix-style), simplify it.

For example,  
path = "/home/", => "/home"  
path = "/a/./b/../../c/", => "/c"  
click to show corner cases.

Corner Cases:
Did you consider the case where path = "/../"?  
In this case, you should return "/".  
Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".

In this case, you should ignore redundant slashes and return "/home/foo".
#code
```
/*
精简目录
note:遇到.则表示当前，遇到..则表示回到父目录，根目录的父目录还是/
栈处理
Input:	"/abc/..."
Output:	"/abc/..."
*/
public class Solution {
    public String simplifyPath(String path) {
    	Stack<String> stack = new Stack<String>();
    	String[] split = path.split("/");
    	for(String str : split) {
    		if(!str.equals(".") && !str.isEmpty()) {
    			if(str.equals("..")) {
    				if(!stack.isEmpty())
    					stack.pop();
    			} else {
    				stack.push(str);
    			}
    		}
    	}
    	StringBuilder sb = new StringBuilder();
    	if(stack.isEmpty()) sb.insert(0, "/");
    	else {
        	while(!stack.isEmpty()) {
        	    sb.insert(0, stack.pop());
        	    sb.insert(0, "/");
        	}
    	}
    	return sb.toString();
    }
}
```