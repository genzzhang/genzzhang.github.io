---
layout: post
title: leetcode:【math】Max Points on a Line
date: 2015-05-02 15:17
categories: 算法
tags: leetcode math
---

# 问题描述 
```
Given n points on a 2D plane, find the maximum number of points that lie on the same straight line.
```

# code

```
/*
在一条直线上最多有几个点。
一个点和斜率可唯一确定一条直线。
首先，这条直线肯定经过某个点。一个点一个点遍历，选最多的点。
固定某点，在和其他点，斜率相同则表示同一条直线。统计经过某点Max Points on a Line
*/
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    public int maxPoints(Point[] points) {
        if(points.length < 3) return points.length;
        int max = 0;
        for(int i = 0;i<points.length;i++) {
            //统计过i点最多的直线数目
            Map<Double,Integer> map = new HashMap<Double,Integer>();
            int maxValue = 0;
            int maxK = 0; //斜率无穷大数目
            int duplicate = 0; //完全一样的
            for(int j = 0;j<points.length;j++) {
                //同一个数，直接返回
                if(i == j) continue;
                //[[1,1],[1,1],[2,2],[2,2]]
                else if(points[i].x == points[j].x && points[i].y == points[j].y) {
                    duplicate++;
                }
                else {
                    //斜率无穷大
                    if(points[i].x == points[j].x)
                        maxK++;
                    //其他正常情况
                    else {
                        double k = (double)(points[i].y - points[j].y)/(points[i].x - points[j].x);
                        if(map.containsKey(k)) {
                            int value = map.get(k);
                            map.put(k,value+1);
                        }
                        else {
                            map.put(k,1);
                        }
                    }
                }
            }
            //统计最大
            Iterator<Double> it = map.keySet().iterator();
            while(it.hasNext()) {
                double db = it.next();
                maxValue = Math.max(maxValue,map.get(db));
            }
            maxValue = Math.max(maxValue,maxK)+duplicate+1;
            max = Math.max(max,maxValue);
        }
        return max;
    }
}
```