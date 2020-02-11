# 1. Two Sum
## 题

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice

from：leetcode-1

## 解

### 方法一：哈希表
- 思路：
  - 查表匹配。若有，返回；若无，需求加入表中。
- 时间复杂度：
  - o(n)
- 空间复杂度：
  - o(n)
- 结果:
  - 用时：64%(16ms)
  - 内存：12%
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> res;
        map<int,int> m;
        for(int i=0;i<nums.size();i++){
            if(m.count(nums[i])){
                res.push_back(m[nums[i]]);
                res.push_back(i);
                break;
            }
            m[target-nums[i]]=i;
        }
        return res;
    }
};
```

## 记

- 简单题，leetcode第一题，但是知识点不少