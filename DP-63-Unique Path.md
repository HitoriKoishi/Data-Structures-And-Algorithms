You are given an `m x n` integer array `grid`. There is a robot initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

An obstacle and space are marked as `1` or `0` respectively in `grid`. A path that the robot takes cannot include **any** square that is an obstacle.

Return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The testcases are generated so that the answer will be less than or equal to `2 * 109`.

```cpp
class Solution {
   public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int n = obstacleGrid.size(), m = obstacleGrid[0].size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        dp[1][1] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (obstacleGrid[i - 1][j - 1] == 1)
                    dp[i][j] = 0;
                else
                    dp[i][j] = max(dp[i - 1][j] + dp[i][j - 1], dp[i][j]);
            }
        }
        return dp[n][m];
    }
};

```

与[题221](DP-221-Maximal Square.md)不同的是，该题目并非试图找出整个矩形中的最优解，而在询问整个题目所有可能的路径，这使得我们不用维护一个ans值来更新最优解，而是dp矩阵最终收敛的右下角的值

对于判断路径数量，首先根据题目中机器人只能每次只能向下或者向右走一格的条件来看，dp的更新只用依据上侧或左侧的值即可。对于路径数量的初始化和更新来说，将机器人所在点的值更新为1，表明机器人到自己所在的格子有一条路径，交汇点的路径数量为上侧或左侧路径数量之和与自身值的最大值，即（`dp[i][j] = max(dp[i - 1][j] + dp[i][j - 1], dp[i][j]);`）。如此将整个矩形遍历后在最右下点可以得到top-left corner到bottom-right corner所有可能的路径数量

为了节省空间复杂度，我们还可以把dp数组优化为一维