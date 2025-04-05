You are given an integer array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return *the **maximum** profit you can achieve*.

```cpp
class Solution {
   public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        int n = prices.size();
        int min_price = prices[0];
        for (int i = 0; i < n; i++) {
            if (i + 1 < n && prices[i + 1] < prices[i]) {
                profit += prices[i] - min_price;
                min_price = prices[i + 1];
            }
            min_price = min(min_price, prices[i]);
        }
        profit += prices[n - 1] - min_price;
        return profit;
    }
};
```

维护一个区间内最小价格，然后在价格无法再上升时卖掉，每个区间产生最利润的综合就是整体的最大利润