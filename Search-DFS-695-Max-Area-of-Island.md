You are given an `m x n` binary matrix `grid`. An island is a group of `1`'s (representing land) connected **4-directionally** (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The **area** of an island is the number of cells with a value `1` in the island.

Return *the maximum **area** of an island in* `grid`. If there is no island, return `0`.

```cpp
class Solution {
   public:
    int dfs(vector<vector<int>>& grid, int i, int j) {
        int n = grid.size(), m = grid[0].size();
        if (i < 0 || i >= n || j < 0 || j >= m || grid[i][j] == 0) {
            return 0;
        }
        grid[i][j] = 0;
        return (1 + dfs(grid, i + 1, j) + dfs(grid, i - 1, j) +
                dfs(grid, i, j + 1) + dfs(grid, i, j - 1));
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) ans = max(ans, dfs(grid, i, j));
            }
        }
        return ans;
    }
};
```

简单的DFS

**无需保存原图中的数据，所以在DFS可以直接将符合条件的点覆写为0避免循环调用，而无需再创建一个visited数组**

