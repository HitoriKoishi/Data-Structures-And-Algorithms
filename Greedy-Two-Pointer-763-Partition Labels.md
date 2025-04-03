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

> **在处理数组前，统计一遍信息（如频率、个数、第一次出现位置、最后一次出现位置等）可以使题目难度大幅降低。**

**频率，个数适用于在给定字母出现个数或频率下的区间搜索**

**第一次出现位置，最后一次出现位置则适用于锚定字母出现的范围**

该题可以使用最后一次出现位置来解决，可以看到在题目案例中每段子串中出现字母的种类是固定的，也就是说字串所涵盖所有字母最后一次出现位置的最大值就可以锚定整个字串最后再哪里与其余字串分隔开

借助该思路，记录原字符串中所有字母的最后一次出现位置，并使用start和end指针标定字串的开始和结束位置，每次遇到一个字母时更新end，直到遍历指针与end会合，表明之前出现的所有字母之后再不会出现（该子串最大程度上涵盖了相同种类的字符），这点就是子串间的分隔点，记录该字串的大小并更新start指针位置