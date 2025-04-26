Given an integer array `nums` and an integer `k`, return *the* `kth` *largest element in the array*.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

```cpp
class Solution {
   public:
    int findKthLargest(vector<int>& nums, int k) {
        int pivot = rand() % nums.size();
        int pivot_val = nums[pivot];
        vector<int> larger, equal, smaller;
        for (auto num : nums) {
            if (num > pivot_val) {
                larger.push_back(num);
            } else if (num < pivot_val) {
                smaller.push_back(num);
            } else {
                equal.push_back(num);
            };
        }
        if (k < larger.size()) {
            return findKthLargest(larger, k);
        }
        if (k > larger.size() + equal.size()) {
            return findKthLargest(smaller, k - larger.size() - equal.size());
        }
        return pivot_val;
    }
};
```

因为传统的快速排序会导致超时，只能用这样空间换时间的做法，**将比枢轴位置大的、小的、相等的数字都存进对应的数组中，通过数组的大小来判断第k大的数会出现在哪个数组中，然后将该数组进行递归运算**

但也有一些直接使用快排题解也能通过~~特别奇怪~~，如：

```cpp
class Solution {
   private:
    int quickSortKth(vector<int>& nums, int low, int high, int k) {
        int mid = low + (high - low) / 2;
        swap(nums[mid], nums[high]);
        int partition = nums[high];
        int left = low, right = high;
        while (left < right) {
            while (left < right && nums[left] >= partition) left++;
            while (left < right && nums[right] <= partition) right--;
            if (left < right) swap(nums[left], nums[right]);
        }
        swap(nums[left], nums[high]);

        if (left == k - 1) return nums[left];
        if (left < k - 1) return quickSortKth(nums, k, left + 1, high);
        return quickSortKth(nums, k, low, left - 1);
    };

   public:
    int findKthLargest(vector<int>& nums, int k) {
        return quickSortKth(nums, 0, nums.size() - 1, k);  
    }
};
```

一种Hoare方案，是快排的逆序排序

有一个点，left最后停留的位置一定是右侧首个小于切分值的元素，这是为什么？

```cpp
while (left < right && nums[left] <= partition) left++;
while (left < right && nums[right] >= partition) right--;
```

若循环代码改成这样一种类似 Lomuto 分区方案，那么因此，left 最后停留的位置，其指向的元素 nums[left]相对于切分值 partition 来说，是**大于或等于** partition 的。更准确地说，它是**从左边数第一个大于 partition 的元素**的位置（或者是 left 和 right 相遇的位置）。

交换时应该与nums[left-1]交换

