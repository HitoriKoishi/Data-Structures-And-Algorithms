There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return *the minimum number of candies you need to have to distribute the candies to the children*.

```cpp
class Solution {
   public:
    int candy(vector<int>& ratings) {
        int ans = 0;
        int s = ratings.size();
        vector<int> candy(s, 1);
        for (int i = 0; i < s - 1; i++) {
            if (ratings[i + 1] > ratings[i]) candy[i + 1] = candy[i] + 1;
        }
        for (int i = s - 1; i > 0; i--) {
            if (ratings[i - 1] > ratings[i])
                candy[i - 1] = max(candy[i - 1], candy[i] + 1);
        }
        return accumulate(candy.begin(), candy.end(), 0);
    }
};
```

存在比较关系的贪心策略并不一定需要排序。虽然这一道题也是运用贪心策略，但我们只需要简单的两次遍历即可：把所有孩子的糖果数初始化为 1；

- 先从左往右遍历一遍，如果右边孩子的评分比左边的高，则右边孩子的糖果数更新为左边孩子的糖果数加 1；

- 再从右往左遍历一遍，如果左边孩子的评分比右边的高，且左边孩子当前的糖果数不大于右边孩子的糖果数，则左边孩子的糖果数更新为右边孩子的糖果数加 1。

通过这两次遍历，分配的糖果就可以满足题目要求了。这里的贪心策略即为，在每次遍历中，只考虑并更新相邻一侧的大小关系。