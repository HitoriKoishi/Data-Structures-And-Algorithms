There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the **height above sea level** of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is **less than or equal to** the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return *a **2D list** of grid coordinates* `result` *where* `result[i] = [ri, ci]` *denotes that rain water can flow from cell* `(ri, ci)` *to **both** the Pacific and Atlantic oceans*.

[原题]([417. 太平洋大西洋水流问题 - 力扣（LeetCode）](https://leetcode.cn/problems/pacific-atlantic-water-flow/description/))

```cpp
class Solution {
   public:
    void dfs(vector<vector<int>>& grid, vector<vector<bool>>& oceans, int i,
             int j) {
        if (i < 0 || j < 0 || i >= grid.size() || j >= grid[0].size()) {
            return;
        }
        if (oceans[i][j]) {
            return;
        }
        oceans[i][j] = true;
        if (i - 1 >= 0 && grid[i - 1][j] >= grid[i][j])
            dfs(grid, oceans, i - 1, j);
        if (i + 1 < grid.size() && grid[i + 1][j] >= grid[i][j])
            dfs(grid, oceans, i + 1, j);
        if (j - 1 >= 0 && grid[i][j - 1] >= grid[i][j])
            dfs(grid, oceans, i, j - 1);
        if (j + 1 < grid[0].size() && grid[i][j + 1] >= grid[i][j])
            dfs(grid, oceans, i, j + 1);
    }

    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int n = heights.size(), m = heights[0].size();
        vector<vector<int>> ans;
        vector<vector<bool>> Pacific(n, vector<bool>(m, false));
        vector<vector<bool>> Atlantic(n, vector<bool>(m, false));
        for (int i = 0; i < n; i++) {
            dfs(heights, Pacific, i, 0);
            dfs(heights, Atlantic, i, m - 1);
        }
        for (int j = 0; j < m; j++) {
            dfs(heights, Pacific, 0, j);
            dfs(heights, Atlantic, n - 1, j);
        }
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (Pacific[i][j] && Atlantic[i][j]) {
                    ans.push_back({i, j});
                }
            }
        }
        return ans;
    }
};
```

如果我们对所有的位置进行搜索，那么在不剪枝的情况下复杂度会很高，这需要一定的逆向思维，**题目中说哪些节点可以同时流到两个大洋，但做的时候我们需要考虑从两个大洋开始逆流，可以共同流到的节点有哪些，这样我们只需要对矩形四条边进行搜索**