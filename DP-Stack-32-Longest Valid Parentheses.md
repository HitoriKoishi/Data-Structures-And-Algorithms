Given a string containing just the characters `'('` and `')'`, return *the length of the longest valid (well-formed) parentheses* *substring*.

```cpp
class Solution {
   public:
    int longestValidParentheses(string s) {
        int n = s.size();
        if (n < 2) return {};
        int ans = 0;
        vector<int> dp(n + 1, 0);
        stack<int> check;
        int length = 0;
        for (int i = 1; i <= n; i++) {
            if (s[i - 1] == '(') {
                check.push(i);
            } else {
                if (!check.empty()) {
                    int past = check.top();
                    check.pop();
                    dp[i] = max(dp[i], dp[past - 1] + i - past + 1);
                    ans = max(ans, dp[i]);
                }
            }
        }
        return ans;
    }
};
```

每次通过
