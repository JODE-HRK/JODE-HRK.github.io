---
title: Ardenia
tags:
  - Leeicode
  - 前缀积

---

## Leetcode 238 除自身以外的积

考虑前缀后缀积，对于每一个数，其答案为该数左边之积乘以该数右边之积

但是求两个缀积的时间复杂度均为O(n)，在这里，我们只求一个后缀积，对于前缀积可以在寻求答案的时候进行推进查找

见代码：

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int len = nums.size();
        vector <int> output(len);
        int back[len+1];//后缀
        back[len] = 1;//初始为1
        for(int i=len-1;i>=0;i--){
            back[i]=back[i+1]*nums[i];
        }
        int front=1;
        for(int i=0;i<len;i++)
            output[i]=(back[i+1]*front),front*=nums[i];//front代表前缀，在不断更新
        return output;
    }
};
```



