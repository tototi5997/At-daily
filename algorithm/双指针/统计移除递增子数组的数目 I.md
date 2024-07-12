[2970. 统计移除递增子数组的数目 I](https://leetcode.cn/problems/count-the-number-of-incremovable-subarrays-i/description/)

移除一个数组的子数组之后，这个数组是一个严格递增的数组，求满足条件的子数组总数

思路 1: 两次遍历，统计所有满足条件的子数组

- 实现一个方法用来判断一个数组去掉 l::r 之后是否严格递增

```ts
function isIncreasing(nums, l, r) {
  // 遍历数组，当在范围 l::r 之内时直接跳过
  // 从 1 开始遍历，因为需要比较 i 和 i - 1
  for (let i = 1; i < nums.length; i++) {
    if (i >= l && i <= r + 1) {
      continue;
    }
    if (nums[i] <= nums[i - 1]) {
      return false;
    }
  }

  //遍历完成之后可以确保 0::i j::n 之间是递增的
  // 还需要处理边界值，nums[i] 也是小于 nums[j]的
  if (l - 1 >= 0 && r + 1 < nums.length && nums[r + 1] <= nums[l - 1]) {
    return false;
  }

  return true;
}
```

- 两次遍历，找到所有满足条件的可能

```ts
const incremovableSubarrayCount = (nums) => {
  let count = 0;

  for (let i = 0; i < nums.length; i++) {
    for (let j = i; j < nums.length; j++) {
      if (isIncreasing(i, j)) count++;
    }
  }

  return count;
};
```
