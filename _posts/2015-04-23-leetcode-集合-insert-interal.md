---
layout: post
title: leetcode:【集合】Insert Interval  
date: 2015-04-23 22:55
categories: 算法
tags: leetcode collection
---

# 问题描述 Insert Interval  

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

>Example 1:  
Given intervals [1,3],[6,9], insert and merge [2,5] in as [1,5],[6,9].    
Example 2:  
Given [1,2],[3,5],[6,7],[8,10],[12,16], insert and merge [4,9] in as [1,2],[3,10],[12,16].   
> 
This is because the new interval [4,9] overlaps with [3,5],[6,7],[8,10].

# code

```
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
 
 /*
 https://leetcodenotes.wordpress.com/2013/10/19/insert-interval/
 思路清晰比什么都重要
 先插入，再合并删除
 */
public class Solution {
    public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        int index = binaryByStart(intervals, newInterval);
        intervals.add(index, newInterval);
        Iterator<Interval> it = intervals.iterator();
        Interval pre = it.next();
        while(it.hasNext()) {
        	Interval now = it.next();
        	boolean remove = false;
        	if(within(pre,now)) remove = true;
        	else if(overlap(pre,now)) {
        		pre.end = now.end;
        		remove = true;
        	} 
        	if(remove) it.remove();
        	else pre = now;
        }
        return intervals;
    }
    
    public int binaryByStart(List<Interval> intervals, Interval newInterval) {
    	int lo = 0,hi = intervals.size()-1;
    	int mid = 0;
    	while(lo <= hi) {
    		mid = (hi+lo)/2;
    		if(intervals.get(mid).start <= newInterval.start) {
    			lo = mid+1;
    			continue;
    		} else if(intervals.get(mid).start > newInterval.start) {
    			hi = mid-1;
    			continue;
    		}
    	}
    	return lo;
    }
    
    private boolean overlap(Interval a,Interval b) {
    	return a.end >= b.start;
    }
    private boolean within(Interval a,Interval b) {
    	return a.end >= b.end;
    }
}
```