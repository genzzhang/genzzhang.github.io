---
layout: post
title: leetcode:【深度优先搜索】与Restore IP Addresses
date: 2015-04-25 15:11
categories: 算法
tags: leetcode tree dfs
---

# 问题描述   
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:  
Given "25525511135",

return ["255.255.11.135", "255.255.111.35"]. (Order does not matter)

# code

```
/*
将字符串转化为4个数字，每个数字不能超过256，同时注意010
思路就是dfs：
每次三个选择：选择1个字符，2个字符还是3个字符
同时检查合法性
*/
public class Solution {
    public List<String> restoreIpAddresses(String s) {
        List<String> lists = new ArrayList<String>();
        int[] startList = new int[5];
        startList[0] = 0;
        //第0次三种选择count=0
        dfs(s,lists,0,1,0,startList);
        dfs(s,lists,0,2,0,startList);
        dfs(s,lists,0,3,0,startList);
        return lists;
        
    }
    private void dfs(String s,List<String> lists,int start,int end,int count,int[] startList) {
    	if(count > 3) {
        	if(start != s.length()) return;
        	StringBuilder sb = new StringBuilder();
        	sb.append(s.substring(startList[0], startList[1]));
        	sb.append('.');
        	sb.append(s.substring(startList[1], startList[2]));
        	sb.append('.');
        	sb.append(s.substring(startList[2], startList[3]));
        	sb.append('.');
        	sb.append(s.substring(startList[3]));
        	lists.add(sb.toString());
        	return;
        }
    	//count+1;
    	startList[++count] = end;
    	if(end <= s.length() && isVaild(s, start, end)) {
    		if(count < 4) {
    		    //前4次选择count=1,2,3，包括0
        		dfs(s,lists,end,end+1,count,startList);
        		dfs(s,lists,end,end+2,count,startList);
        		dfs(s,lists,end,end+3,count,startList);
    		} else {
    		    //4次都已选择，接下来dfs就是判断并打印。
    		    //在判断是否打印中，由于判断的缘故，这里就一次
    			dfs(s,lists,end,end+1,count,startList);
    		}
    	} else {
    		return;
    	}    
    }
    //判断是否合法
    private boolean isVaild(String s,int start,int end) {
    	String sb = s.substring(start,end);
        if(sb.isEmpty()) return false;
        if(sb.matches("[0][0-9]+")) return false;
        if( Integer.parseInt(sb)< 256)  return true;
        return false;
    }
}
```