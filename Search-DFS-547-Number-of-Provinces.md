There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return *the total number of **provinces***.

```cpp
class Solution {
   public:
    void dfs(vector<vector<int>>& isConnected, int i, vector<bool>& visited) {
        for (int j = 0; j < isConnected.size(); j++) {
            if (isConnected[i][j] == 1 && !visited[j]) {
                visited[j] = true;
                dfs(isConnected, j, visited);
            }
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int ans = 0;
        int n = isConnected.size();
        vector<bool> visited(n, false);
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                visited[i] = true;
                dfs(isConnected, i, visited);
                ans++;
            }
        }
        return ans;
    }
};
```

**DFS，只是循环不用二维，依旧使用DFS来判断连通的所有节点**
