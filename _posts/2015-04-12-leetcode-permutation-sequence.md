---
layout: post
title: leetcode:Permutation Sequence
date: 2015-04-12 11:39
categories: 算法
tags: leetcode
---

#问题描述
The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

"123"
"132"
"213"
"231"
"312"
"321"
Given n and k, return the k<sup>th</sup> permutation sequence.

Note: Given n will be between 1 and 9 inclusive.

#code
```
/*
n个元素的排列总数是n!。
getPermutation(n, k)，对于(n, k)=(1, 1)
在下面的分析中，让k的范围是0 <= k < n!。（题目代码实际上是1<=k<=n!)
可以看到一个规律，就是这n！个排列中，第一位的元素总是(n-1)!一组出现的，
也就说如果p = k / (n-1)!，那么排列的最开始一个元素一定是arr[p]。
这个规律可以类推下去，在剩余的n-1个元素中逐渐挑选出第二个，第三个，
...，到第n个元素。程序就结束。
 */
public String getPermutation(int n, int k) {
    List<Integer> nums = new ArrayList<Integer>();
    for(int i = 1;i<=n;i++)
        nums.add(i);
    StringBuilder str = new StringBuilder();
    int kMul = 1;
    for(int i =1;i<n;i++) 
        kMul *= i;
    int quoty = k-1; //余数
    for(int i = n-1;i>=0;i--) {
        int kQuoty = quoty/kMul;
        int remainder = quoty%kMul;
        quoty = remainder;
        kMul /= (i != 0?i:1);
        str.append(nums.get(kQuoty));
        nums.remove(nums.get(kQuoty));
    }
    return str.toString();
}
```