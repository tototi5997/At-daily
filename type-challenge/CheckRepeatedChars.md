[实现 CheckRepeatedChars](https://github.com/type-challenges/type-challenges/blob/main/questions/09142-medium-checkrepeatedchars/README.zh-CN.md)

判断一个 string 类型中是否有相同的字符

```ts
type CheckRepeatedChars<'abc'>   // false
type CheckRepeatedChars<'aba'>   // true

```

思路:

- 使用模板字符串的方式解析字符串，`${infer L}${infer R}`
- 分析 L ，判断 L 字母是否在后续 R 中出现
- R extends `${string}${L}${string}`
- 如果存在说明重复，如果不存在，继续判断后半部分即可

```ts
type CheckRepeatedChars<T extends string> = T extends `${infer L}${infer R}`
  ? R extends `${string}${L}${string}`
    ? true
    : CheckRepeatedChars<R
  : false
```
