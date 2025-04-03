You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part. For example, the string `"ababcc"` can be partitioned into `["abab", "cc"]`, but partitions such as `["aba", "bcc"]` or `["ab", "ab", "cc"]` are invalid.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return *a list of integers representing the size of these parts*.

```cpp
class Solution {
   public:
    vector<int> partitionLabels(string s) {
        vector<int> ans;
        vector<int> letter_last(26, 0);
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            letter_last[s[i] - 'a'] = i;
        }
        for (int i = 0; i < s.length(); i++) {
            end = max(end, letter_last[s[i] - 'a']);
            if (end == i) {
                ans.push_back(end - start + 1);
                start = i + 1;
            }
        }
        return ans;
    }
};
```

这题要解决的问题就是每段字串之间的分隔点如何判断

> 在处理数组前，统计一遍信息（如频率、个数、第一次出现位置、最后一次出现位置等）可以使题目难度大幅降低。

频率，个数适用于在