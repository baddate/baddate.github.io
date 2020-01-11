---
title: Solutions of Leetcode
date: 2019-09-23
lastmod: 2019-09-23
weight: 1
url:
    /solutions/leetcode
tags:
    - Leetcode
categories:
    - 2019-09
description: "some leetcode's solutions"
draft: false

---

## 3Sum

__problem description:__

<https://leetcode.com/problems/3sum/>

__thought:__

1. we can define 3 pointers called `i`,`j`,`k`.
2. sort this array(up).
3. keep the `k`, move `i`&`j` from `k+1`to`nums.length()-1`
4. init `i=k+1` & `j=nums.length()-1`

Then we can get 3 positoins:

1. we should loop the array when `i<j`
2. nums[k] > 0, then we can break the loop and return.
3. if we find the same numbers, we should skip it if it isn't the 1st. Or we should add it to `result set`.

__solution:__

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>> res;
        for(int k = 0;k < nums.size();k++)
        {
            if(nums[k] > 0)
                break;
            if(k > 0 && nums[k] == nums[k-1])
                continue;
            int i = k + 1;
            int j = nums.size() - 1;
            while(i<j)
            {
                int temp = nums[k] + nums[i] + nums[j];
                if(temp < 0){
                   while(i < j && nums[i] == nums[++i]);
                }
                else if(temp>0){
                   while(i < j && nums[j] == nums[--j]);
                }
                else{
                    res.push_back(vector<int>{nums[k], nums[i], nums[j]});
                    while(i < j && nums[i] == nums[++i]);
                    while(i < j && nums[j] == nums[--j]);
                }
            }
        }
        return res;
    }
};
```

## Longest Substring Without Repeating Characters

__problem description:__

<https://leetcode.com/problems/longest-substring-without-repeating-characters/>

__thought:__

Using the `Sliding Window`

_p.s. maybe we can use hashmap for optimizing._

__solution:__

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if(!s.size())
            return 0;
        int i=0, j=0, len=0, max=0;
        for(;i<s.size()&&len<s.size()-i;i++){//reduce loop size
            int flag=0;
            for(j=i;j<i+len&&flag!=1;j++){
                if(s[j]==s[i+len]){
                    flag=1;
                }
            }
            if(flag==0){
                i--;
                len++;
            }else{
                len=len-((j-1)-i);
                i=j-1;
            }
        max = max<len?len:max;
        }
        return max;
    }
};
```

__note:__

1. `a<b?a:b` when `a<b` is true,return a.

2. _3 c++ string comparation methods:_

```c++
s1 == s2
s1.c_str() == s2.c_str()
strcmp(s1.c_str(),a2.c_str()) == 0
```
