You are given an array of people, `people`, which are the attributes of some people in a queue (not necessarily in order). Each `people[i] = [hi, ki]` represents the `ith` person of height `hi` with **exactly** `ki` other people in front who have a height greater than or equal to `hi`.

Reconstruct and return *the queue that is represented by the input array* `people`. The returned queue should be formatted as an array `queue`, where `queue[j] = [hj, kj]` is the attributes of the `jth` person in the queue (`queue[0]` is the person at the front of the queue).

[原题](https://leetcode.cn/problems/queue-reconstruction-by-height/)

 ```cpp
 class Solution {
    public:
     vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
         sort(people.begin(), people.end(), [](vector<int>& a, vector<int>& b) {
             return a[0] == b[0] ? a[1] <= b[1] : a[0] > b[0];
         });
         list<vector<int>> ans;
         for (const auto& p : people) {
             auto pos = ans.begin();
             advance(pos, p[1]);
             ans.insert(pos, p);
         }
         return vector<vector<int>>(ans.begin(), ans.end());
         
         //上方代码与下方功能等效，上方代码性能更优一些
         
 		// vector<vector<int>> ans;
         // for (const auto& p : people) {
         //     ans.insert(ans.begin() + p[1], p); 
         // }
         // return ans;
     }
 };
 ```

<s>数据结构用vector也可以，但是插入操作会更费时间一点</s>

**更多地利用lambda函数，将身高从高到低排序，前方有几个大于等于自己身高的人从低到高排序**

之后根据从高到低的顺序直接插入即可）<s>娘的没想到这么简单</s>