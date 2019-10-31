---
layout: post
title:  动态规划
date:   2019-10-31 10:13:5 +0800
categories: 手撕代码
tags: c++ LeetCode algorithm
author: gzf
excerpt: 手撕代码之动态规划
mathjax: true
---

* content
{:toc}

## 动态规划介绍



## 例题 
*例题均来自[LeetCode](https://leetcode-cn.com/)*


### 1. 最大整除子集 [☄](https://leetcode-cn.com/problems/largest-divisible-subset/)
给出一个由无重复的正整数组成的集合，找出其中最大的整除子集，子集中任意一对 (Si，Sj) 都要满足：Si % Sj = 0 或 Sj % Si = 0。
如果有多个目标子集，返回其中任何一个均可。
> 示例 1:
>
> 输入: [1,2,3]
> 
> 输出: [1,2] (当然, [1,3] 也正确)

#### 思路
首先对数组排序
设定状态 `dp[i]`: 代表以nums[i]为结尾时，符合条件的子集的最大长度
`last[i]`: 代表当以`nums[i]`为结尾时，符合整除条件的上一个数在`nums`中的位置。

状态转换：
使用两层循环，当`nums[i]%nums[j]==0`(符合题目要求的整除)并且`dp[i]<=dp[j]`(如果`dp[i]>dp[j]`,即使把`dp[i]`放在`dp[j]`这个子集后面也不会是`dp[i]`变长)，`dp[i]=dp[j]++;`同时更新`last[i]=j;`
#### 代码
```cpp

```