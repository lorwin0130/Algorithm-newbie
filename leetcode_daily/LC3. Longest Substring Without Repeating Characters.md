# 3. Longest Substring Without Repeating Characters

## 题

给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

## 解

### 方法一：
- 思路：
  - 双指针
- 时间复杂度：
  - o(n)
- 空间复杂度：
  - < o(n)
- 结果:
  - 用时：38%（40ms）
  - 内存：64%（9.3MB）
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int i = 0, j = 0, res = 0;
        unordered_set<char> dict;
        while(j < s.size()){
            if(dict.count(s[j]) == 1){
                while(s[i] != s[j]){
                    dict.erase(s[i]);
                    ++i;
                }
                ++i;
            }
            dict.insert(s[j]);
            ++j;
            res = max(res, j - i);
        }
        return res;
    }
};
```

## 记
<!-- 
基础：@basic
重点：@important
记忆：@memory
易错：@warning
待办：@todo
模板：@template
 -->

- 2020.05.02 by lorwin