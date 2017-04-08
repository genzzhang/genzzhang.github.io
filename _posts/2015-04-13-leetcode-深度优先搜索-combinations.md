---
layout: post
title: leetcode:【深度优先搜索】Combinations
date: 2015-04-13 23:08
categories: 算法
tags: leetcode dfs
---

#问题描述
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,  
If n = 4 and k = 2, a solution is:  

[  
　[2,4],  
  　[3,4],  
  　[2,3],  
  　[1,2],  
  　[1,3],  
  　[1,4],  
]

#code
```
/*
第一个思路就是dfs
n,k
(XXX...XXX).size == k
every X = 1.2.3...n 用mark标记是否已经访问
Submission Result: Time Limit Exceeded 
this method's output:
combine(4, 2)
12=13=14=21=23=24=31=32=34=41=42=43
note:[1,2] == [2,1]
按照题目的意思应该要标记起始点
12=13=14=23=24=34
*/
public class Solution {
	public List<List<Integer>> list;
	public List<Integer> listItem;
	public boolean[] mark;
	public int nn;
	public int kk;
	
    public List<List<Integer>> combine(int n, int k) {
    	list = new ArrayList<List<Integer>>();
    	listItem = new ArrayList<Integer>();
    	mark = new boolean[n];
    	kk = k;
    	nn = n;
    	getListItem(0);
    	return list;
    }
    
    private void getListItem(int step) {
    	if(step==kk) {
    		List<Integer> temp = new ArrayList<Integer>();
    		for(int i = 0;i<listItem.size();i++)
    			temp.add(listItem.get(i));
    		list.add(temp);
    		return;
    	}
    	for(int i = 0;i<nn;i++) {
    		if(!mark[i]) {
    			mark[i] = true;
    			if(listItem.size() == kk)
    				listItem.set(step, i+1);
    			else {
    				listItem.add(i+1);
    			}
    			getListItem(step+1);
    			mark[i] = false;
    		}
    	}
    };
}

/*
对于这种固定长度解法
step=0,有何种选择，选择的范围：起点终点，下一步的选择范围是什么
直到step=k-1,共进行了k步，
当step=k,也就是超出固定长度则处理return
Submission Result: Accepted
*/	
public class Solution {
    public List<ArrayList<Integer>> combine(int n, int k) {
    	List<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();
    	if(k<=0 || n<1) return null;
    	getListItem(1,n,0,list,new int[k]);
    	return list;
    }
    
    private void getListItem(int start, int end, int step, List<ArrayList<Integer>> list, int[] path) {
    	if(step==path.length) {
    		ArrayList<Integer> listItem = new ArrayList<Integer>();
    		for(int i = 0;i<step;i++)
    			listItem.add(path[i]);
    		list.add(listItem);
    		return;
    	} else {
    		for(int i = start; i<= end;i++) {
    			path[step] = i;
    			getListItem(i+1, end, step+1, list, path);
    		}
    	}
    };
}
```