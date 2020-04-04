# 8. String to Integer (atoi)

## 题

请你来实现一个 atoi 函数，使其能将字符串转换成整数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：

如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：

本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/string-to-integer-atoi

## 解

### 方法一：
- 思路：
  - 直接判断，遇到数字加入
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
    int myAtoi(string str) {
        // 空格跳过
        int i=0, flag=1;
        long long res=0;
        while(str[i]==' ') i++;
        // 如果第一个非空字符为+ -
        if(str[i]=='+') flag=1, i++;
        else if(str[i]=='-') flag=-1, i++;
        // 找最大连续数字
        while(str[i]-'0'>=0 && str[i]-'0'<=9){
            // 假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。
            res = res*10 + str[i]-'0';
            if(flag==1 && res>=INT_MAX) return INT_MAX;
            if(flag==-1 && res>=(long long)-1*INT_MIN) return INT_MIN;
            i++;
        }
        // 若函数不能进行有效的转换时，请返回 0 。
        return (int)flag*res;
    }
};
```

### 方法二：有限自动机(@memory)
- 思路：
  - 用有限自动机的思想来编写程序，先写出
- 时间复杂度：
  - o(n)
- 空间复杂度：
  - o(1)
- 结果:
  - 用时：12%（20ms）
  - 内存：100%
```
//有限自动机类
class Automaton {
    //当前状态，状态转移列表，计算下一状态
    #define START 0
    #define SIGN 1
    #define NUMBER 2
    #define END 3
    int state=START;
    unordered_map<int, vector<int>> turn={
        {START,{START,SIGN,NUMBER,END}},
        {SIGN,{END,END,NUMBER,END}},
        {NUMBER,{END,END,NUMBER,END}},
        {END,{END,END,END,END}}
    };
    int next_state(char c){
        if(c==' ') return 0;
        if(c=='+' || c=='-') return 1;
        if(isdigit(c)) return 2;
        return 3;
    }
public:
    //记录结果（绝对值，符号），状态转移
    long long res=0;
    int sign=1;
    bool get_res(char c){
        state=turn[state][next_state(c)];
        if(state==SIGN && c=='-') sign=-1;
        else if(state==NUMBER){
            res=res*10 + sign*(c-'0');
            if(sign==1 && res>INT_MAX) {res=INT_MAX; return true;} //提前结束
            if(sign==-1 && res<INT_MIN) {res=INT_MIN; return true;} //提前结束
        }else if(state==END) return true; //提前结束
        return false;
    }
};
class Solution {
public:
    int myAtoi(string str) {
        Automaton at;
        for(char c:str){
            if(at.get_res(c)) break;
        }
        return (int)at.res;
    }
};
```

## 记

- 2020.04.03 by lorwin