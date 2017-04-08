---
layout: post
title: leetcode:【动态规划】Jump Game II 
date: 2015-04-12 23:29
categories: 算法
tags: leetcode
---

#问题描述
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

For example:
Given array A = [2,3,1,1,4]

The minimum number of jumps to reach the last index is 2. (Jump 1 step from index 0 to 1, then 3 steps to the last index)

#code
```
/*
常规DP
Submission Result: Time Limit Exceeded
定义F(i):到第i个元素的最小跳数
能抵达i位置的跳转位置为:j=0,1,2,3...i-1中A[j]+j>=i的j
则F(i)=min(F(j))+1,j=0,1,....i-1,且A[j]+j>=i
 */
public static int jump(int[] A) {
	if(A == null ||  A.length == 0) return -1;
	int len = A.length;
	int[] f = new int[len];
	f[0] = 0;
	for(int i = 1;i<len;i++) 
		f[i] = Integer.MAX_VALUE;		
	for(int i = 1;i<len;i++)
		for(int j = 0;j<i;j++)
			if((A[j]+j)>=i)
				f[i] = Math.min(f[i], f[j]+1);
	return f[len-1];				
}
/*
 进一步优化
 倒推F(i)=min(F(j))+1,j=0,1,....i-1,且A[j]+j>=i
 顺推:从当前的往后推
 已知F(i)，则F(i+1)到F(i+A[i])均可以刷新一遍，取min(F(j),F(i)+1)
 Submission Result: Time Limit Exceeded
 */
public static int jump(int[] A) {
	if(A == null ||  A.length == 0) return -1;
	int len = A.length;
	int[] f = new int[len];
	f[0] = 0;
	for(int i = 1;i<len;i++) 
		f[i] = Integer.MAX_VALUE;		
	for(int i = 0;i<len-1;i++)
		for(int j = 1;j<=A[i];j++) {
			if(i+j>len-1) continue;
			f[i+j] = Math.min(f[i+j], f[i]+1);
		}				
	return f[len-1];				
}
/*
进一步处理，
已知F(i)，则F(i+A[i])到F(i+1)倒，
遇到min则break，
因为F(i+A[i])到F(i+1)只是与F(i)有关，F(i)递增
Submission Result: Accepted
*/
public static int jump(int[] A) {
	if(A == null ||  A.length == 0) return -1;
	int len = A.length;
	int[] f = new int[len];
	f[0] = 0;	
	for(int i = 1;i<len;i++) 
		f[i] = Integer.MAX_VALUE;		
	for(int i = 0;i<len-1;i++)
		for(int j = A[i];j>0;j--) {
			if(i+j>len-1 || f[i+j]<f[i]+1)  continue;
			f[i+j] = f[i]+1;
		}				
	return f[len-1];				
}

/*
从头开始遍历，找到第一个可以跳到最后一个的位置，记第一个跳转的位置为T
接着从头开始遍历，找到第一个可以调到T，
依次循环直到T=0
012345678
第一个到达8位置的是5,012345和0123456+步数，后者一定大于等于前者，且到达后者的一定能到达前者
Submission Result: Accepted
 */
public static int jump(int[] A) {
	if(A == null ||  A.length == 0) return -1;
	int len = A.length -1;
	int step = 0;
	while(len > 0) {
		for(int j = 0;j<len;j++) {
			if((A[j]+j)>=len) {
				step++;
				len = j;
				break;
			}
		}
	}
	return step;			
}

/*
状态:每步最远的位置
step=0 f(0) = 0;
step=1 f(1) = f(0)+A[f(0)];
f(step) = max(j+A[j])   j位第step-1可以的起点，f(step-2)<j<=f(step-1)注意范围
Submission Result: Accepted
 */
public static int jump(int[] A) {
	if(A == null ||  A.length == 0) return -1;
	int len = A.length;
	int last = 0;
	int curr = 0;
	int step = 0;
	for(int i = 0;i<len;i++) {
		if(i>last) {
			last = curr;
			step++;
		}
		curr = Math.max(curr, i+A[i]);
	}
	return step;			
}

```