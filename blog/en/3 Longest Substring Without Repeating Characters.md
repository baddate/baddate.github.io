@def title = "3. Longest Substring Without Repeating Characters"
@def published = "24 September 2019"
@def tags = ["Leetcode", "Solution"]


__problem description:__

[https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

__thought:__

Using the `Sliding Window` method.

*p.s. maybe we can use hashmap for optimizing.*

__solution:__

```cpp
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

2. *3 c++ string comparation methods*

```cpp
s1 == s2
s1.c_str() == s2.c_str()
strcmp(s1.c_str(),a2.c_str()) == 0
```
