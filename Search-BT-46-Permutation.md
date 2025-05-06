Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

[原题]([46. 全排列 - 力扣（LeetCode）](https://leetcode.cn/problems/permutations/description/))

```cpp
class Solution {
   public:
    vector<vector<int>> ans;
    void backtrace(vector<int>& nums, vector<int>& cur, vector<bool>& visited) {
        if (cur.size() == nums.size()) {
            ans.push_back(cur);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (visited[i] == true) continue;
            visited[i] = true;
            cur.push_back(nums[i]);
            backtrace(nums, cur, visited);
            cur.pop_back();
            visited[i] = false;
        }
    }

    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> cur;
        vector<bool> visited(nums.size(), false);
        backtrace(nums, cur, visited);
        return ans;
    }
};
```

- 标注当前元素已访问，将当前元素加入现有状态
- 回溯搜索下一层
- （上一层返回后）将当前元素剔除现有状态，标注当前元素未访问

回溯主要就是由这个逻辑来构成的