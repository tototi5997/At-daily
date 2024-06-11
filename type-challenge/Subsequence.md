[实现 Subsequence](https://github.com/type-challenges/type-challenges/blob/main/questions/08987-medium-subsequence/README.md)

实现给定一个唯一元素数组，返回所有可能的子序列。 子序列是可以通过删除一些元素或不删除元素而不改变剩余元素的顺序从数组派生的序列。

```ts
type A = Subsequence<[1, 2]>; // [] | [1] | [2] | [1, 2]
```

思路：

- 返回的是联合类型，可以使用`T[number]`
- 联合类型每个元素都为元组，应该对原数组进行遍历处理
- 返回联合类型中除了单个元素，还包括剩余元素，说明每次遍历需要加入剩余元素

```ts
type Subsequence<T extends any[]> = T extends [infer L, ...infer R] ? [L, ...Subsequence<R>] | Subsequence<R> : [];
```
