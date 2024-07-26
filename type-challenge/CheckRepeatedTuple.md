[实现 CheckRepeatedChars](https://github.com/type-challenges/type-challenges/blob/main/questions/27958-medium-checkrepeatedtuple/README.md)

实现类型 `CheckRepeatedChars<T>` ，它将返回类型 T 是否包含重复的成员

```ts
type CheckRepeatedTuple<[1, 2, 3]>   // false
type CheckRepeatedTuple<[1, 2, 1]>   // true
```

判断元组中是否存在重复元素，遍历元组每一个元素，用一个额外的辅助数组用来保存遍历过的元素

每个元素判断是否和剩余元素重复或者和已经遍历过的元素重复，有重复立刻返回结果

遍历结束，如果没有重复，返回结果

```ts
type CheckRepeatedTuple<T extends unknow[], C extends unknow[]> = T extends [infer L, ...infer R]
  ? L extends C[number] | R[number]
    ? true
    : ChekRepeatedTuple<R, [...C, L]>
  : false;
```
