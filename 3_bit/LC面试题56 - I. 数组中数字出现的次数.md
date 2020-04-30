# 面试题56 - I. 数组中数字出现的次数

## 题

一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。

## 解

### 方法一：
- 思路：
  - 位运算，所有数的异或sum为结果要找的两个数的异或，从sum的一个1位进行0-1分组，可以将两个只出现一次的数分到两组，而且出现两次的数分到同一组中。
  - 然后异或两组，得到结果的两个数
- 时间复杂度：
  - o(n)
- 空间复杂度：
  - o(1)
```
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int sum=0, sel=1, a=0, b=0;
        for(int n:nums) sum^=n;
        while((sum&sel)==0) sel<<=1;
        for(int n:nums){
            if(n&sel) a^=n;
            else b^=n;
        }
        return {a,b};
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
 -->

- @class:bit
- 2020.04.28 by lorwin