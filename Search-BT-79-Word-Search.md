Given an `m x n` grid of characters `board` and a string `word`, return `true` *if* `word` *exists in the grid*.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

[原题]([79. 单词搜索 - 力扣（LeetCode）](https://leetcode.cn/problems/word-search/description/))

```cpp
class Solution {
   public:
    vector<pair<int, int>> direction = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    bool backtrace(vector<vector<char>>& board, string word,
                   vector<vector<bool>>& visited, int i, int j, int pos) {
        if (pos == word.size()) return true;
        for (auto orien : direction) {
            int nx = i + orien.first;
            int ny = j + orien.second;
            if (nx < 0 || nx >= board.size() || ny < 0 ||
                ny >= board[0].size() || visited[nx][ny]) {
                continue;
            }
            if (board[nx][ny] == word[pos]) {
                visited[nx][ny] = true;
                if (backtrace(board, word, visited, nx, ny, pos + 1))
                    return true;
                visited[nx][ny] = false;
            }
        }
        return false;
    }

    bool exist(vector<vector<char>>& board, string word) {
        int n = board.size(), m = board[0].size();
        vector<vector<bool>> visited(n, vector<bool>(m, false));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (board[i][j] == word[0]) {
                    visited[i][j] = true;
                    if (backtrace(board, word, visited, i, j, 1)) return true;
                    visited[i][j] = false;
                }
            }
        }
        return false;
    }
};
```

正确判断何时应该`board[nx][ny] == word[pos]`来**修正字符串匹配逻辑**

和`if (backtrace(board, word, visited, i, j, 1)) return true;`的**提前返回优化**是关键