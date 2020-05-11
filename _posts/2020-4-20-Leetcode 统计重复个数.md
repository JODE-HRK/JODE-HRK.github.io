---
title: Leetcode 统计重复个数
tags:
  - Leetcode
  - 字符串
  - 循环节
  - C++
---

MMP，这题卡了我几天了，到头来还是看的题解。

循环节我一直在用KMP来写，被循环节绕进去了，其实用不着KMP，只需要一个循环就可以了

```cpp
class Solution
{
public:
    int getMaxRepetitions(string s1, int n1, string s2, int n2)
    {
        map<int, pair<int, int>> S;//这个记录之前这种情况是否出现过
        if (n1 == 0)
            return 0;
        int s1cnt = 0, index = 0, s2cnt = 0;//cnt是计数器，记有多少个，index代表在s2中的位置
        S.clear();
        pair<int, int> pre_loop;
        pair<int, int> in_loop;
        while (true)
        {
            s1cnt++;
            for (auto ch : s1)
            {
                if (ch == s2[index])
                {
                    index++;
                    if (index == s2.length())
                        s2cnt++, index = 0;
                }
            }
            if (s1cnt == n1)
                return s2cnt / n2;
            if (S.find(index) != S.end())//当这种情况之前出现过，说明循环节来了
            {
                pair<int, int> p = S[index];//获得之前的情况（因为之前有一段可能不被包含在循环节里面）
                int s1cnt_prime = p.first, s2cnt_prime = p.second;
                pre_loop = p;//循环节之前
                in_loop = make_pair(s1cnt - s1cnt_prime, s2cnt - s2cnt_prime);//循环节里面
                break;
            }
            else
                S[index] = make_pair(s1cnt, s2cnt);
        }
        int ans = pre_loop.second + (n1 - pre_loop.first) / in_loop.first * in_loop.second;
        //这个ans记录有多少个s2串
        int rest = (n1 - pre_loop.first) % in_loop.first;//最后面还有多少剩余
        for (int i = 0; i < rest; i++)
            for (auto ch : s1)
            {
                if (ch == s2[index])
                {
                    index++;
                    if (index == s2.length())
                        ans++, index = 0;
                }
            }//最后在算一下后面有多少个s2就可以了
        return ans / n2;
    }
};
```

