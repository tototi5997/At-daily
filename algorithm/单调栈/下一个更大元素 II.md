[503.下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/description/)

给定一个循环数组 nums （ nums[nums.length - 1] 的下一个元素是 nums[0] ），返回 nums 中每个元素的 下一个更大元素 。

解法 1:

暴力解法，遍历数组，找到比当前元素大的下一个元素，当遍历一圈没有找到，返回 -1

```js
const nextGreaterElements = (nums) => {
  let res = [];
  let n = nums.length;
  for (let i = 0; i < n; i++) {
    let next = i + 1 <= n - 1 ? i + 1 : 0;

    // 找下一个比 nums[i] 大的数
    while (nums[next] <= nums[i]) {
      if (next === i) break; // 没找到
      if (next === n - 1) {
        next = 0;
      } else next++;
    }

    if (next === i) {
      res.push(-1);
      continue;
    }

    res.push(nums[next]);
  }

  return res;
};
```

解法 2:

- 构建一个单调栈，从下往上单调递减
- 遍历数组，如果单调栈中没有元素直接加入（第一个）
- 当单调栈中有元素的时候，比较当前元素和栈定元素大小
- 当前元素大于栈顶元素，说明找到了下一个大的元素，保存当前元素索引，栈顶元素出栈，直到当前元素可以作为栈顶元素
- 当前元素小于栈顶元素，直接入栈

```js
const nextGreaterElements = (nums) => {
  const n = nums.length;
  const ans = new Array(n).fill(-1);
  const st = [];
  // 将两个数组拼接到一起，模拟循环查找的过程，这一题之需要查找2次即可
  for (let i = 0; i < 2 * n - 1; i++) {
    const x = nums[i % n];
    while (st.length && x > nums[st[st.length - 1]]) {
      ans[st[st.length - 1]] = x;
      st.pop();
    }
    st.push(i % n);
  }
  return ans;
};
```
