### [LeetCode-1 (Easy)](https://leetcode.com/problems/two-sum/)	**Two Sum**

题意：给定一个数组和一个目标值（target），返回数组中加和等于目标值的两数下标。

注意：输入值保证只会有一个结果，并且一个值只能使用一次。

示例：

```
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

思路：

利用一个哈希表，哈希表的key值存储对应的值，哈希表的value存储值在数组中的下标，对数组进行遍历，因为求是否存在两数的和为目标值，所以可以通过使用目标值减去当前遍历到的数，来得到另一个数，如果该值在map中已存在，则从map中取出该值的下标值，与当前遍历到的数值的下标一起返回，即为结果，如果没有从map中获取到需要的值，则将当前遍历到的值存入map中。

代码：

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(target - nums[i])) {
            return new int[]{i, map.get(target - nums[i])};
        }
        map.put(nums[i], i);
    }
    throw new IllegalArgumentException("No two sum solution");
}
```

时间复杂度：需要遍历数组，所以时间复杂度为 O(N)。

空间复杂度：用到了哈希表，所以额外空间复杂度也为 O(N)。

----

### [LeetCode-11 (Medium)](https://leetcode.com/problems/container-with-most-water/)	Container With Most Water

题意：给定一非负数组，数组每一项的值为一边 ，每一项之间的距离为一边形成容器，求容器最大的容量。

示例:

```html
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```

**解法一：暴力搜索**

当前边与余下所有边组成容器，求最大容量。根据木桶原理，容器盛水取决于较小边，所以遍历时，需要取组成容器的两边中的较小值（Math.min(height[i], height[j])）与两边的距离相乘，得到容量。

代码：

```java
public int maxArea(int[] height) {
    int maxArea = 0;
    for (int i = 0; i < height.length; i++) {
        for (int j = i; j < height.length; j++) {
            maxArea = Math.max(maxArea, Math.min(height[i], height[j]) * (j - i));
        }
    }
    return maxArea;
}
```

*时间复杂度：O($N^2$)*

*空间复杂度：O(1)*



**解法二：双指针**

定义两个指针，指向数组首尾，通过移动指向的较小的边的指针来获取更大的可能容量。

代码：

```java
public int maxArea(int[] height) {
    int maxArea = 0;
    int l = 0, r = height.length - 1;
    while (l < r) {
        int h = Math.min(height[l], height[r]);
        maxArea = Math.max(maxArea, h * (r - l));
        if (height[l] <= height[r]){
            l++;
        } else r--;
    }
    return maxArea;
}
```

*时间复杂度：O(N)*

*空间复杂度：O(1)*

----

### [LeetCode-15 (Medium)](https://leetcode.com/problems/3sum/)	**3Sum**

题意：给定一个数组，求数组中所有的*a*, *b*, *c* 和为0（*a* + *b* + *c* = 0）的组合。

示例：

```html
Given array nums = [-1, 0, 1, 2, -1, -4],
A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

思路：

先将数组进行排序，然后遍历数组每一项，遍历每一项时，使用双指针，扫描剩余数组部分，如果扫描到的三值之和为0，则加入返回列表，遍历过程中注意跳过重复值。

代码：

```java
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> ans = new ArrayList<>();
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 2; i++) {
        int j = i + 1;
        int k = nums.length - 1;
        while (j < k) {
            if (nums[j] + nums[k] == -nums[i]) {
                ans.add(Arrays.asList(nums[i], nums[j], nums[k]));
                j++;
                k--;
                while (j < k && nums[j] == nums[j - 1]) j++; //避免结果中有重复
                while (j < k && nums[k] == nums[k + 1]) k--;
            } else if (nums[j] + nums[k] > -nums[i]) {
                k--;
            } else {
                j++;
            }
        }
    }
    return ans;
}
```

*时间复杂度：O($N^2$)*

*空间复杂度：O(1)*

----

### [LeetCode-26 (Easy)](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)	**Remove Duplicates from Sorted Array**

题意：给定一个排序数组，**原地**删除重复元素，返回删除后数组长度。

示例：

```
Given nums = [0,0,1,1,1,2,2,3,3,4],

Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.

It doesn't matter what values are set beyond the returned length.
```

代码：

```java
public int removeDuplicates(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    int j = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != nums[j]) {
            j++;
            nums[j] = nums[i];
        }
    }
    return j + 1;
}
```

*时间复杂度：O(N)*

*空间复杂度：O(1)*

----



###  [LeetCode-283 (Easy)](https://leetcode.com/problems/move-zeroes/)	**Move Zeroes**

题意：给定一个数组，将数组中的0值都移动到数组末尾，并且保持非0值的原有顺序不变。

注意：1.不能复制数组，只能原地操作。2.最小化操作的次数。

示例:

```html
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

解法一：冒泡排序

遍历数组，遇到0值，就将其往数组末尾冒泡。

代码：

```java
public void moveZeroes(int[] nums) {
    for (int i = nums.length - 1; i >= 0; i--) {
        for (int j = 0; j < i; j++) {
            if (nums[j] == 0) {
                int tmp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = tmp;
            }
        }
    }
}
```

*时间复杂度：O($N^2$)*

*空间复杂度：O(1)*



解法二：一次遍历

遍历一次数组，将非0值按序移动到数组前面部分，数组剩余部分补0。

代码：

```java
public void moveZeroes1(int[] nums) {
    int k = 0;
    for (int i = 0; i < nums.length; i++) {
        if (nums[i] != 0) {
            nums[k++] = nums[i];
        }
    }
    for (int i = k; i < nums.length; i++) {
        nums[i] = 0;
    }
}
```

*时间复杂度：O(N)*

*空间复杂度：O(1)*

----



### [LeetCode-169 (Easy)](https://leetcode.com/problems/move-zeroes/)	**Majority Element**

题意：给定一个大小为n的数组，找到其中的众数。众数是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

可以假设数组是非空的，并且给定的数组总是存在众数。

示例:

```html
Input: [3,2,3]
Output: 3
```

**解法一：**暴力搜索

代码：

```java
public int majorityElement(int[] nums) {
    int count = nums.length / 2;
    for (int i = 0; i < nums.length; i++) {
        int k = 0;
        for (int j = i; j < nums.length; j++) {
            if (nums[j] == nums[i]) {
                k++;
            }
        }
        if (k > count) {
            return nums[i];
        }
    }
    return -1;
}
```

*时间复杂度：O(n ^ 2)*

*空间复杂度：O(1)*

**解法二：** 哈希表

代码：

```java
public int majorityElement(int[] nums) {
    Map<Integer, Integer> map = new LinkedHashMap<Integer, Integer>();
    for (int i = 0; i < nums.length; i++) {
        if (map.containsKey(nums[i])) {
            Integer k = map.get(nums[i]);
            map.put(nums[i], ++k);
        } else {
            map.put(nums[i], 1);
        }
    }
		Integer maxKey = 0;
    Integer maxValue = 0;
    for (Map.Entry entry : map.entrySet()) {
        if ((Integer) entry.getValue() > maxValue) {
            maxValue = (Integer) entry.getValue();
            maxKey = (Integer) entry.getKey();
        }
    }
    return maxKey;
}
```

*时间复杂度：O(n)*

*空间复杂度：O(n)*

----

### [LeetCode-162 (Medium)](https://leetcode.com/problems/find-peak-element/)	**Find Peak Element**

题意：峰值元素是指比它左右相邻数值都要大的数，给定一个数组nums，其中可能包含多个峰值元素，请找出其中一个。

示例一：

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

示例二：

```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

解法一：线性查找

```java
public int findPeakElement(int[] nums) {
    for (int i = 0; i < nums.length - 1; i++) {
        if (nums[i] > nums[i + 1])
            return i;
    }
    return nums.length - 1;
}
```

解法二：二分查找

```java
public int findPeakElement(int[] nums) {
    int l = 0, r = nums.length - 1;
    while (l < r) {
    int mid = (l + r) / 2;
        if (nums[mid] > nums[mid + 1])
            r = mid;
        else
            l = mid + 1;
    }
    return l;
}
```

*时间复杂度：O(logN)*

*空间复杂度：O(1)*