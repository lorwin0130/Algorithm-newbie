# 面试题57 - II. 和为s的连续正数序列 LCOF

## 题

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof

## 解

### 方法一：暴力枚举
- 思路：
  - 暴力遍历查找，上界是target/2
- 时间复杂度：
  - o(n*sqrt(n))
- 空间复杂度：
  - o(1)
- 结果:
  - 用时：5%（560ms）
  - 内存：100%
```
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> res;
        for(int i=1;i<=target/2;i++){
            int sum=0,j;
            vector<int> tmp;
            for(j=i;sum<target;j++) sum+=j,tmp.push_back(j);
            if(sum==target) res.push_back(tmp);
        }
        return res;
    }
};
```

### 方法二：数学方法
- 思路：
  - 应用求根公式
- 时间复杂度：
  - o(n)
- 空间复杂度：
  - o(1)
- 结果:
  - 用时：54%（8ms）
  - 内存：100%
```
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> res;
        for(long i=1;i<=target/2;i++){
            long delta=1-4*i+4*i*i+8*target;
            if(delta>0){
                long sq=(long)sqrt((float)delta);
                if(sq*sq==delta && (sq-1)%2==0){
                    vector<int> tmp;
                    for(int j=i;j<=(sq-1)/2;j++) tmp.push_back(j);
                    res.push_back(tmp);
                }
            }
        }
        return res;
    }
};
```

### 方法三：滑动窗口！！！（重点）
- 思路：
  - **双指针滑动窗口!!!**
- 时间复杂度：
  - o(n)
- 空间复杂度：
  - o(1)
- 结果:
  - 用时：85%（4ms）
  - 内存：100%
```
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        vector<vector<int>> res;
        int i=1,j=1;
        while(i<=target/2){
            int sum=(i+j)*(j-i+1)/2;
            if(sum<target) j++;
            else if(sum>target) i++;
            else{
                vector<int> tmp;
                for(int k=i;k<=j;k++) tmp.push_back(k);
                res.push_back(tmp);
                i++;j++;
            }
        }
        return res;
    }
};
```

## 记

- (@memony) **学习并记忆滑动窗口的思想！！！**
  - 滑动窗口的重要性质是：窗口的左边界和右边界永远只能向右移动，而不能向左移动。这是为了保证滑动窗口的时间复杂度是 O(n)。如果左右边界向左移动的话，这叫做“回溯”，算法的时间复杂度就可能不止 O(n)。
  - [来源](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/shi-yao-shi-hua-dong-chuang-kou-yi-ji-ru-he-yong-h/)
- 2020.03.06 by lorwin