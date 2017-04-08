---
layout: post
title: leetcode:查找Find Minimum in Rotated Sorted Array
date: 2015-04-16 17:32
categories: 算法
tags: leetcode 查找
---

# 问题描述 Find Minimum in Rotated Sorted Array

Suppose a sorted array is rotated at some pivot unknown to you beforehand.  

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.

# code

```
public class Solution {
    public int findMin(List<Integer> nums) {
        int left = 0;
        int right = nums.size()-1;
        while(left < right) {
            int mid = left + (right - left)/2;
            //判断是否是正序
            if(nums.get(left)<=nums.get(mid) && nums.get(mid)<=nums.get(right)) {
                break;
            }
            //考虑逆序的情况，选择min出现在左区间还是右区间
            else {
                int midValue = (nums.get(left)+nums.get(right))/2;
                if(nums.get(mid) >= midValue)
                    left = mid+1;
                else {
                    right = mid;
                }
                    
            }
        }
        return nums.get(left);
    }
}


/*
二分查找，logN，根据中点分为两个(left--mid),(mid+1--right)区间,循环查找min在那个区间
首先判断是否rotated，不是则第一个left即是min
如果是，则接着判断min在左还是右区间，根据midValue>rightValue判断左右那个是rotated的
*/
public class Solution {
    public int findMin(List<Integer> nums) {
        int left = 0;
        int right = nums.size()-1;
        while(nums.get(left) > nums.get(right)) {
            int mid = (left+right)/2;
            if(nums.get(mid) > nums.get(right))
                left = mid +1;
            else
                right = mid;
        }
        return nums.get(left);
    }
}
```

# 问题描述 Find Minimum in Rotated Sorted Array II  

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

The array may contain duplicates.

# code

```
/*
3
3313
31333
前提是递增
可能是rotated的情况是：左比右大或者相等，此时注意死循环
分三种情况，大于、小于和等于
等于的时候，相当于遍历
*/
public class Solution {
    public int findMin(List<Integer> nums) {
        int left = 0;
        int right = nums.size()-1;
        while(nums.get(left) >= nums.get(right) && left < right) {
            int mid = (left+right)/2;
            if(nums.get(mid) > nums.get(right))
                left = mid +1;
            else if(nums.get(mid) < nums.get(right)) 
                right = mid;
            else {
                left++;
            }
        }
        return nums.get(left);
    }
}
```