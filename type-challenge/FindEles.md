[实现 FindEles](https://github.com/type-challenges/type-challenges/blob/main/questions/09898-medium-zhao-chu-mu-biao-shu-zu-zhong-zhi-chu-xian-guo-yi-ci-de-yuan-su/README.zh-CN.md)

找出目标数组中只出现过一次的元素

```ts
type preview = FindEles<[1, 2, 2, 3, 3, 4, 5, 6, 6, 6]>; // [1, 4, 5]
```

思路：

- 遍历元素，判断当前元素是否出现在后续元素中
- 如果出现了，说明当前元素重复，保存到重复数组中
- 如果没有出现，说明当前元素在后续不重复
- 判断当前元素是否出现在重复数组中（在前面出现过）
- 如果出现了，说明当前元素在前面重复，跳过
- 如果没有出现，说明当前元素唯一，保存到结果数组中
- 遍历完成，返回结果数组

```ts
type FindEles<T extends any[], M extends any[] = [], U extends any[] = []> = T extends [infer L, ...infer R]
  ? L extends R[number]
    ? FindEles<R, [...M, L], U>
    : L extends M[number]
    ? FindEles<R, M, U>
    : FindEles<R, M, [...U, L]>
  : U;
```
