The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return *all distinct solutions to the **n-queens puzzle***. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

[原题](https://leetcode.cn/problems/n-queens/description/)

```cpp
class Solution {
   public:
    void backtrace(vector<vector<string>> &ans, vector<string> &chess,
                   vector<bool> &diag1, vector<bool> &diag2,
                   vector<bool> &colunm, int row) {
        int n = colunm.size();
        int i = row;
        if (row == chess.size()) {
            ans.push_back(chess);
            return;
        }
        for (int j = 0; j < n; j++) {
            if (!colunm[j] && !diag1[i - j + n - 1] && !diag2[i + j]) {
                colunm[j] = diag1[i - j + n - 1] = diag2[i + j] = true;
                chess[i][j] = 'Q';
                backtrace(ans, chess, diag1, diag2, colunm, i + 1);
                chess[i][j] = '.';
                colunm[j] = diag1[i - j + n - 1] = diag2[i + j] = false;
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> ans;
        vector<bool> diag1(2 * n - 1, false);
        vector<bool> diag2(2 * n - 1, false);
        vector<bool> colunm(n, false);
        vector<string> chess(n, string(n, '.'));
        backtrace(ans, chess, diag1, diag2, colunm, 0);
        return ans;
    }
};
```

要注意**对角线的表示方法**和**棋盘的字符串初始化**

传递引用的时间空间复杂度要比传递形参要快许多