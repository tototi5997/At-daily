[实现 FirstUniqueCharIndex](https://github.com/type-challenges/type-challenges/blob/main/questions/09286-medium-firstuniquecharindex/README.md)

给定一个字符串 s，找到其中第一个不重复的字符并返回其索引。如果不存在，则返回-1。

```ts
FirstUniqueCharIndex<"leetcode">; // 0
```

解法 1:

思路：

- 遍历字符串，IndexOf 判断当前字符的位置
- LastIndexOf 判断当前字符的最后一个位置
- 如果两个相等，说明当前字符唯一

```ts
type StrToArr<S extends string> = S extends `${infer L}${infer R}` ? [L, ...StrToArr<R>] : [];
type FirstUniqueCharIndex<T extends string, S extends string = T> = T extends `${infer L}${infer R}`
  ? IndexOf<StrToArr<S>, L> extends LastIndexOf<StrToArr<S>, R>
    ? IndexOf<StrToArr<S>, L>
    : FirstUniqueCharIndex<R, S>
  : -1;
```

解法 2:

思路：

- 遍历字符串，当前字符是否在统计数组中
- 如果在，当前字符是否在后续字符中出现
- 如果没有出现，返回统计数组的长度
- 如果出现了，继续遍历后续字符，统计数组加入当前字符
- 如果在当前数组中，继续遍历后续字符，统计数组加入当前字符

```ts
type FirstUniqueCharIndex<T extends string, U extends any[] = []> = T extends `${infer L}${infer R}`
  ? L extends U[number]
    ? FirstUniqueCharIndex<R, [...U, L]>
    : R extends `${string}${L}${string}`
    ? FirstUniqueCharIndex<R, [...U, L]>
    : U["length"]
  : -1;
```

```ts
type FirstUniqueCharIndex<T extends string, U extends any[] = []> = T extends `${infer L}${infer R}`
  ? R extends `${string}${L}${string}`
    ? FirstUniqueCharIndex<R, [...U, L]>
    : L extends U[number]
    ? FirstUniqueCharIndex<R, [...U, L]>
    : U["length"]
  : -1;
```
