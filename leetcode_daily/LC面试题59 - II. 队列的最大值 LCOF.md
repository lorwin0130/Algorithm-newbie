# 面试题59 - II. 队列的最大值 LCOF

## 题

请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof

## 解

### 方法一：
- 思路：
  - 除了需要一个队列实现本身的队列功能，对于实现取队列最大值的功能，还需要维持一个双端队列。双端队列记录一个最大值的递减序列
    - push：每次进行push操作，将push值value与双端队列的尾部值back进行比较。如果value>back,则从双端队列中pop(back),继续比较；如果value<=back,则push(value)进双端队列和队列
    - pop:每次进行pop操作，pop队列的队首，将pop值与双端队列的首部进行比较，如果相等，则pop双端队列的队首。
    - max:双端队列的队首就是max_value
- 时间复杂度：
  - max:o(1)
  - push:o(n),平均o(1)
  - pop:o(1)
- 空间复杂度：
  - o(n)
- 结果:
  - 用时：97%（132ms）
  - 内存：100%
```
class MaxQueue {
    queue<int> q;
    deque<int> d;
public:
    MaxQueue() {}
    
    int max_value() {
        if(q.empty()) return -1;
        return d.front();
    }
    
    void push_back(int value) {
        while(!d.empty() && d.back()<value) d.pop_back();
        d.push_back(value);
        q.push(value);
    }
    
    int pop_front() {
        if(q.empty()) return -1;
        int val=q.front();q.pop();
        if(d.front()==val) d.pop_front();
        return val;
    }
};
```

## 记

- 2020.03.07 by lorwin