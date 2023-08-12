# 回溯算法刷题
**回溯是穷举的一种**
但是无法用叠加for循环来实现
所以使用递归，每次递归里实现一个for，就可以叠加for使用
## 回溯的模式
由于回溯就是一种递归，所以要满足递归三要素
1. 返回值和参数
2. 终止条件
3. 单次递归要做的事 

想捋清楚递归要做的事，就可以通过树状图来理解
___
## 题目：
### 从n个数中选出k个数的组合，即Cnk
[题目链接](https://leetcode.cn/problems/combinations/)
> 给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

解题思路：
 ![树状图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123195223940.png)

- 可以看到，单层的处理逻辑就是从剩余的（除被取过）的数中取一个，然后进入下一层；完成深度遍历后，再回退回来取本层的其余。
- 终止条件则是，当取够了k个数，就终止。
- 所以我们要维护一个数组，记录单次深度遍历取的结果。

那么，一次深度遍历就构成一次递归；用for将递归包起来，就能将整个树状图表达出来。

除此之外，我们还需要考虑递归的返回值、参数和对结果的影响。

- 每次递归都维护一个数组，完成一次深度遍历后，将数组作为结果拿出来
- 如果回退一次，可以直接把上一次pushback的结果再pop出来即可，不需要从头再构建一次
- 每进到下一层，for的起点就变成了上一层加入的值+1，比如第二层加入了2，第三层就可以加入3和4

因此，大致的代码结构可以写成
```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        _combine_heler(n, k, 1);
        return result;
    }

private:
    vector<vector<int>> result;
    vector<int> path;
    void _combine_heler(int n, int k, int start) {
       if (path.size() == k) {
           result.push_back(path);
           return;
       }
       for (int i = start; i <= n; i++) {
           path.push_back(i);
           _combine_heler(n, k, i + 1);
           path.pop_back();
       }
    }
};
