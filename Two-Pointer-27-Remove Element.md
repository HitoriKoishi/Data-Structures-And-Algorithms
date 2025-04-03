Given an integer array `nums` and an integer `val`, remove all occurrences of `val` in `nums` [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm). The order of the elements may be changed. Then return *the number of elements in* `nums` *which are not equal to* `val`.

Consider the number of elements in `nums` which are not equal to `val` be `k`, to get accepted, you need to do the following things:

- Change the array `nums` such that the first `k` elements of `nums` contain the elements which are not equal to `val`. The remaining elements of `nums` are not important as well as the size of `nums`.
- Return `k`.

**Custom Judge:**

The judge will test your solution with the following code:

```markdown
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

If all assertions pass, then your solution will be **accepted**.

---

```cpp
class Solution {
   public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0, right = 0;
        int s = nums.size();
        while (right < s) {
            if (nums[right] == val) {
                right++;
                continue;
            }
            nums[left++] = nums[right++];
        }
        return left;
    }
};
```

该双指针的大体思路是：

- 使用left指针锚定需要替换的数

- 使用right指针遍历整个数组，跳过需要替换的数字
- 同时将无需替换的数字移动至left指针位置，覆盖left位置的数字，最终的数组长度就是left指针指向的数组索引

该种替换的方式可以优化朴素算法下O(n^2)的时间复杂度，一次遍历即可完成替换

**进一步优化，根据题目中返回数组的元素顺序可以变化的条件可以直接从两端对碰，减少重复元素赋值的开销：**

```cpp
class Solution {
   public:
    int removeElement(vector<int>& nums, int val) {
        int s = nums.size();
        int left = 0, right = s - 1;
        while (left <= right) {
            if (nums[left] == val) {
                nums[left] = nums[right];
                right--;
            } else {
                left++;
            }
        }
        return left;
    }
};
```

