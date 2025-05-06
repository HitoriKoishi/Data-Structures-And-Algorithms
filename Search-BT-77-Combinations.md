Given two integers `n` and `k`, return *all possible combinations of* `k` *numbers chosen from the range* `[1, n]`.

You may return the answer in **any order**.

[原题]([77. 组合 - 力扣（LeetCode）](https://leetcode.cn/problems/combinations/description/))

```cpp
class Solution {
   public:
    void backtrace(vector<vector<int>> &ans, vector<int> &cur, int n, int k,
                   int pos) {
        if (cur.size() == k) {
            ans.push_back(cur);
            return;
        }
        for (int i = pos; i < n; i++) {
            cur.push_back(i + 1);
            backtrace(ans, cur, n, k, i + 1);
            cur.pop_back();
        }
    }

    vector<vector<int>> combine(int n, int k) {
        vector<int> cur;
        vector<vector<int>> ans;
        backtrace(ans, cur, n, k, 0);
        return ans;
    }
};
```

记录当前位置的回溯