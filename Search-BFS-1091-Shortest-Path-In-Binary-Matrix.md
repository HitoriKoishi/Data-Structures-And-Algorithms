Given an `n x n` binary matrix `grid`, return *the length of the shortest **clear path** in the matrix*. If there is no clear path, return `-1`.

A **clear path** in a binary matrix is a path from the **top-left** cell (i.e., `(0, 0)`) to the **bottom-right** cell (i.e., `(n - 1, n - 1)`) such that:

- All the visited cells of the path are `0`.
- All the adjacent cells of the path are **8-directionally** connected (i.e., they are different and they share an edge or a corner).

The **length of a clear path** is the number of visited cells of this path.

[原题]([1091. 二进制矩阵中的最短路径 - 力扣（LeetCode）](https://leetcode.cn/problems/shortest-path-in-binary-matrix/))

```cpp
class Solution {
   public:
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int ans = 1;
        int n = grid.size(), m = grid[0].size();
        queue<pair<int, int>> que;
        if (grid[0][0] == 1) return -1;
        que.push({0, 0});
        grid[0][0] = 1;	//初始点也是，要在刚进入队列时就标记为以访问
        vector<pair<int, int>> direction = {{0, 1}, {1, 0},  {0, -1}, {-1, 0},
                                            {1, 1}, {-1, 1}, {1, -1}, {-1, -1}};
        while (!que.empty()) {
            int s = que.size();
            for (int i = 0; i < s; i++) {
                pair<int, int> cur = que.front();
                que.pop();
                //grid[cur.first][cur.second] = 1;	错误的写法，从队列中取出单元格时才标记已访问可能会导致同一个单元格可能被多次加入队列，造成重复计算
                if (cur.first == n - 1 && cur.second == m - 1) {
                    return ans;
                }
                for (auto orien : direction) {
                    int nx = cur.first + orien.first;
                    int ny = cur.second + orien.second;
                    if (nx >= 0 && nx < n && ny >= 0 && ny < m &&
                        grid[nx][ny] == 0) {
                        que.push({nx, ny});
                        grid[nx][ny] = 1;  // 入队时标记为已访问
                    }
                }
            }
            ans++;
        }
        return -1;
    }
    
    //可进一步优化为
    
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();

        // 起点或终点不可达时直接返回
        if (grid[0][0] == 1 || grid[n-1][m-1] == 1) return -1;
        if (n == 1 && m == 1) return 1;  // 起点就是终点

        const vector<pair<int, int>> directions = {
            {0, 1}, {1, 0}, {0, -1}, {-1, 0},
            {1, 1}, {-1, 1}, {1, -1}, {-1, -1}
        };

        queue<pair<int, int>> q;
        q.push({0, 0});
        grid[0][0] = 1;

        int steps = 1;
        while (!q.empty()) {
            int level_size = q.size();

            for (int i = 0; i < level_size; ++i) {
                auto [x, y] = q.front();  // 结构化绑定(C++17)
                q.pop();

                if (x == n-1 && y == m-1) return steps;

                for (const auto& [dx, dy] : directions) {
                    int nx = x + dx, ny = y + dy;

                    if (nx >= 0 && nx < n && ny >= 0 && ny < m && grid[nx][ny] == 0) {
                        q.push({nx, ny});
                        grid[nx][ny] = 1;  // 只在这里标记一次
                    }
                }
            }

            ++steps;
        }
    	return -1;  // 无法到达终点
    }
};
```

BFS的基础题型，注意**严格标记已访问条件，对层级的计数方法**

对于结构性的优化可以再多了解了解，多看看