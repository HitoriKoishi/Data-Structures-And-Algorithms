There are some spherical balloons taped onto a flat wall that represents the XY-plane. The balloons are represented as a 2D integer array `points` where `points[i] = [xstart, xend]` denotes a balloon whose **horizontal diameter** stretches between `xstart` and `xend`. You do not know the exact y-coordinates of the balloons.

Arrows can be shot up **directly vertically** (in the positive y-direction) from different points along the x-axis. A balloon with `xstart` and `xend` is **burst** by an arrow shot at `x` if `xstart <= x <= xend`. There is **no limit** to the number of arrows that can be shot. A shot arrow keeps traveling up infinitely, bursting any balloons in its path.

Given the array `points`, return *the **minimum** number of arrows that must be shot to burst all balloons*.

```cpp
class Solution {
   public:
    int findMinArrowShots(vector<vector<int>>& points) {
        int s = points.size();
        int arrow = 1;
        sort(points.begin(), points.end(),
             [](vector<int>& a, vector<int>& b) { return a[0] < b[0]; });
        int aft = points[0][1];
        for (int i = 1; i < s; i++) {
            if (points[i][0] <= aft) {
                aft = min(aft, points[i][1]);
                continue;
            } else {
                arrow++;
                aft = points[i][1];
            }
        }
        return arrow;
    }
};
```

与[435-non-overlapping-intervals](Greedy-435-non-overlapping-intervals.md)相似，不同点在于435题要求找到不重合区间，而该题要求找到最多的重合区间

思路首先利用lambda函数对区间头从小到大进行排序，与此同时维护一个最小尾部用以判断该区间是否与之前的区间重合；当不重合时，表明下一个气球必须用一根新的箭才能射中，并将最小尾部更新为新区间的尾部