You are given an `n x n` binary matrix `grid` where `1` represents land and `0` represents water.

An **island** is a 4-directionally connected group of `1`'s not connected to any other `1`'s. There are **exactly two islands** in `grid`.

You may change `0`'s to `1`'s to connect the two islands to form **one island**.

Return *the smallest number of* `0`*'s you must flip to connect the two islands*.

[原题]([934. 最短的桥 - 力扣（LeetCode）](https://leetcode.cn/problems/shortest-bridge/description/))

```cpp
class Solution {
   public:
    vector<pair<int, int>> direction = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

    void dfs(vector<vector<int>>& grid, queue<pair<int, int>>& q, int i,
             int j) {
        int n = grid.size(), m = grid[0].size();
        if (i < 0 || i >= n || j < 0 || j >= m || grid[i][j] == 2) return;
        if (grid[i][j] == 0) {
            q.push({i, j});
            return;
        }
        grid[i][j] = 2;
        for (auto orien : direction) {
            int nx = i + orien.first;
            int ny = j + orien.second;
            dfs(grid, q, nx, ny);
        }
    }

    int shortestBridge(vector<vector<int>>& grid) {
        int path = 1;
        bool firstland = false;
        queue<pair<int, int>> q;
        int n = grid.size(), m = grid[0].size();
        for (int i = 0; i < n && !firstland; i++) {
            for (int j = 0; j < m && !firstland; j++) {
                if (grid[i][j] == 1) {
                    dfs(grid, q, i, j);
                    firstland = true;
                }
            }
        }
        while (!q.empty()) {
            int s = q.size();
            for (int i = 0; i < s; i++) {
                pair<int, int> cur = q.front();
                q.pop();

                for (auto orien : direction) {
                    int nx = cur.first + orien.first;
                    int ny = cur.second + orien.second;
                    if (nx >= 0 && nx < n && ny >= 0 && ny < m) {
                        if (grid[nx][ny] == 0) {
                            q.push({nx, ny});
                            grid[nx][ny] = 2;  // 入队时标记为已访问
                        } else if (grid[nx][ny] == 1) {
                            return path;
                        }
                    }
                }
            }
            path++;
        }
        return path;
    }
};
```

实际上是计算两个岛屿之间的最短路径，首先使用DFS将一个岛屿全部标记为2，然后将所有该岛屿邻接的水域加入到BFS的队列中，通过遍历岛屿之间最小的搜索层数来确定两个岛屿之间的最小路径