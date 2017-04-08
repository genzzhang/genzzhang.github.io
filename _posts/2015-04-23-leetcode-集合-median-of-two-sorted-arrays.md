---
layout: post
title: leetcode:【集合】Median of Two Sorted Arrays
date: 2015-04-24 16:49
categories: 算法
tags: leetcode collection
---

# 问题描述 Median of Two Sorted Arrays

There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

# code

```
/*
Median就是中位数，中间的那个数或者中间两个数的平均值
题目中要求int[m]和int[n]两个已经排好序数组中的中位数，
也就是求m+n个有序数中间数，m+n为计数，则刚好取中间，否则取和均值
所以，分类计算。
要求时间复杂度为对数，则类似于二分查找。

解决此题的方法可以依照：寻找一个unioned sorted array中的第k大（从1开始数）的数。
那么如何判断两个有序数组A,B中第k大的数呢？
我们需要判断A[k/2 -1]和B[k-k/2 -1]的大小。【index 从0开始，第k/2个数在k/2-1处】
如果A[k/2-1]==B[k-k/2-1]，那么这个数就是两个数组中第k大的数。
如果A[k/2-1]<B[k-k/2-1], 那么说明A[0]到A[k/2-1]都不可能是第k大的数，所以需要舍弃这一半;
继续从A[k/2--length-1]和B[0--length-1] 剩下中找第k-k/2大数。

对于实际情况中，
从A和B中每次查找分配的数目，肯定不能超过本身的范围。
part1 = Math.min(m,k/2);part2 = k - part1
为了方便处理
if(m > n) return findKth(nums2,start2,end2,nums1,start1,end1,k);
默认每次都是前面的数目少
这样，如果可对比数目为0，则全部在后者
特别注意查找的数目为1，直接比就行。否则0、1分配可能死循环
*/
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int len1 = nums1.length;
        int len2 = nums2.length;
        int k = (len1+len2)/2;
        if((len1+len2)%2 == 1)
            return findKth(nums1,0,len1-1,nums2,0,len2-1,k+1);
        else {
            int sum = findKth(nums1,0,len1-1,nums2,0,len2-1,k)+findKth(nums1,0,len1-1,nums2,0,len2-1,k+1);
            return sum/2.0;
        }
    }
    private int findKth(int[] nums1, int start1, int end1, int[] nums2, int start2, int end2,int k) {
        int m = end1 -start1 + 1;
        int n = end2- start2 + 1;
        //保障前者的数目小于等于后者
        if(m > n) return findKth(nums2,start2,end2,nums1,start1,end1,k);
        if(m == 0) return nums2[start2+k-1];
        if(k == 1) return Math.min(nums1[start1], nums2[start2]);
        
        int part1 = Math.min(m,k/2);
        int part2 = k - part1;
        
        if(nums1[start1+part1-1] < nums2[start2+part2-1])
            return findKth(nums1,start1+part1,end1,nums2,start2,end2,k-part1);
        else if(nums1[start1+part1-1] == nums2[start2+part2-1])
            return nums1[start1+part1-1];
        else {
            return findKth(nums1,start1,end1,nums2,start2+k-part1,end2,part1);
        }
        
    }
}
```