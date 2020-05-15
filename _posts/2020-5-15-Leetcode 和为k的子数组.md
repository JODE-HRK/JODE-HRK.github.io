---
title: Leetcode 和为k的子数组
tags:
  - C++
  - 算法竞赛
  - 动态规划
  - Leetcode
---

## Leetcode 和为k的子数组

题意：给你一个数组和一个数k，要你求这个数组有多少个连续的子数组之和为k。

限制：数组的长度为[1,20000]	数组中元素的范是[-1000,1000],且整数k的范是[-1e7,1e7]

分析：如果是暴力，那就是O(N^2)的时间复杂度，明显会超时，其做法是对每个i进行枚举j，求**j~i**之间的和。

​		记为sum[ i ]-sum[ j-1 ]

​		 那么，我们需要对枚举这个操作进行优化，想想，题目要求有多少段子组数组，那么就是求到i这个位置之前存在多少个和为k的子数组。那么我们只要求sum-k在之前是否存在过即可。对于到sum-k这个位置之前存在过的k，那么肯定中间有一段k-k=0的一段，所以在sum-k这个位置要加上一个1即可。

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int sum=0,ans=0;
        map <int,int> mp;
        mp[0]=1;
        for(int i=0;i<nums.size();i++){
            sum+=nums[i];
            if(mp.find(sum-k)!=mp.end())
                ans+=mp[sum-k];
            mp[sum]++;
        }
        return ans;
    }
};
```

