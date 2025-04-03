You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array `flowerbed` containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return `true` *if* `n` *new flowers can be planted in the* `flowerbed` *without violating the no-adjacent-flowers rule and* `false` *otherwise*.

暴力：

```
```



贪心：

```cpp
class Solution {
   public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int s = flowerbed.size();
        int cnt = 0;
        int prev = -1;
        for (int i = 0; i < s; i++) {
            if (flowerbed[i] == 1) {
                if (prev < 0) {
                    cnt += i / 2;
                } else {
                    cnt += (i - prev - 2) / 2;
                }
                if (cnt >= n) {
                    return true;
                }
                prev = i;
            }
        }
        if (prev < 0) {
            cnt += (s + 1) / 2;
        } else {
            cnt += (s - prev-1) / 2;
        }

        return cnt>=n;
    }
};
```

