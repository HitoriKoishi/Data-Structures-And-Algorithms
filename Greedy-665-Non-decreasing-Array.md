Given an array `nums` with `n` integers, your task is to check if it could become non-decreasing by modifying **at most one element**.

We define an array is non-decreasing if `nums[i] <= nums[i + 1]` holds for every `i` (**0-based**) such that (`0 <= i <= n - 2`).

```cpp
class Solution {
   public:
    bool checkPossibility(vector<int>& nums) {
        int n = nums.size();
        int count = 0;
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] > nums[i + 1]) {
                count++;
                if (count > 1) return false;
                if (i > 0 && nums[i - 1] > nums[i + 1]) {
                    nums[i + 1] = nums[i];
                    // 尝试修复一处不满足非递减性质的地方
                } else {
                    nums[i] = nums[i + 1];
                }
            }
        }
        return count <= 1;
    }
};
```

1. **贪心选择策略**：遍历数组时，当发现 `nums[i] > nums[i+1]`，必须修改一个元素以消除递减。此时有两种选择：
   - 将 `nums[i]` 减小为 `nums[i+1]`（优先考虑，若不影响前面的非递减性）。
   - 将 `nums[i+1]` 增大为 `nums[i]`（当必须保持前面的顺序时采用）。
2. **局部最优决策**：
   - 若 `i > 0` 且 `nums[i-1] > nums[i+1]`，说明修改 `nums[i]` 为 `nums[i+1]` 会导致 `nums[i-1]` 和新的 `nums[i]` 仍可能递减（例如序列 `[3, 4, 2]`），因此必须将 `nums[i+1]` 提升至 `nums[i]`，确保前面部分保持非递减。
   - 否则，将 `nums[i]` 降为 `nums[i+1]`，避免影响后续元素的检查。

**根据局部的上下文特征先作出修改，若修改后仍不满足性质则可认为无法以仅修改一处的方式来满足非递减的条件**