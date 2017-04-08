---
layout: post
title: leetcode:【排序】Merge Intervals
date: 2015-05-05 23:34
categories: 算法
tags: leetcode sort collection
---

# 问题描述   
```
Given a collection of intervals, merge all overlapping intervals.

For example,  
Given [1,3],[2,6],[8,10],[15,18],  
return [1,6],[8,10],[15,18].  
```

# code

```
/*
先排序吧Arrays【数组[]】Collections【链表list】
再一个一个比较吧
*/

/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
    public List<Interval> merge(List<Interval> intervals) {
        if(intervals.size() < 2) return intervals;
        Collections.sort(intervals,new MyComparator());
        Iterator<Interval> it = intervals.iterator();
        Interval now = it.next();
        while(it.hasNext()) {
            Interval next = it.next();
            if(isOverlap(now, next)) {
                now.start = Math.min(now.start,next.start);
                now.end = Math.max(now.end,next.end);
                it.remove();
            } else if(isWithin(now,next))
                it.remove();
            else {
                now = next;
            }
        }
        return intervals;
    }
    
    private boolean isOverlap(Interval a, Interval b) {
        return a.end >= b.start;
    }
    private boolean isWithin(Interval a, Interval b) {
        return a.start <= b.start && a.end >= b.end;
    }
    private class MyComparator implements Comparator<Interval> {
        public int compare(Interval a,Interval b) {
            return a.start-b.start;
        }
    }
}
```