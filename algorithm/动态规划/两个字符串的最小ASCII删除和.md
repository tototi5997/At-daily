[712.两个字符串的最小 ASCII 删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/description/?envType=study-plan-v2&envId=dynamic-programming)

给定两个字符串 s1 和 s2，返回 使两个字符串相等所需删除字符的 ASCII 值的最小和 。

思路：动态规划

- `dp[i][j]` 表示 `s1[0::i]` 之间的字符串和 `s2[0::j]`之间的字符串转换，需要删除的最小 ASCLL 和
- 构建 `dp[m + 1][n + 1]` 其中 m 和 n 分别为 s1 和 s2 的长度
- 初始值，`dp[0][j]` 表示 s2 向空字符串转换，等于 `s2[0::j]` 的 ASCLL 和
- 同理 `dp[i][0]` 表示向 s1 向工字符串转换，等于 `s1[0::i]` 的 ASCLL 和
- 当 s[i] === s[j] 的时候，`dp[i][j] = dp[i - 1][j - 1]`
- 否则，我们需要考虑 `dp[i][j] = min(dp[i - 1][j] + Ascall(s1[i - 1]), dp[i][j - 1] + Ascall(s2[j - 1]))`

以 sea 和 eat 为例：

| i,j | ''  | s    | e   | a   |
| --- | --- | ---- | --- | --- |
| ''  | 0   | s    | se  | sea |
| e   | e   | se   | s   | sa  |
| a   | ea  | sea  | sa  | s   |
| t   | eat | seat | sat | st  |

```js
const minimumDeleteSum = function (s1, s2) {
  const m = s1.length;
  const n = s2.length;
  const dp = new Array(m + 1).fill(0).map(() => new Array(n + 1).fill(0));

  dp[0][0] = 0;

  for (let i = 1; i <= m; i++) {
    dp[i][0] = dp[i - 1][0] + s1[i - 1].charCodeAt();
  }

  for (let j = 1; j <= n; j++) {
    dp[0][j] = dp[0][j - 1] + s2[j - 1].charCodeAt();
  }

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (s1[i - 1] === s2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1];
      } else {
        dp[i][j] = Math.min(dp[i - 1][j] + s1[i - 1].charCodeAt(), dp[i][j - 1] + s2[j - 1].charCodeAt());
      }
    }
  }

  return dp[m][n];
};
```