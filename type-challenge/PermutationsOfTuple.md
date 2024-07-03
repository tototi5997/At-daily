[实现 PermutationsOfTuple](https://github.com/type-challenges/type-challenges/blob/main/questions/21220-medium-permutations-of-tuple/README.md)

给定一个泛型元组类型 T 扩展未知[]，编写一个将 T 的所有排列生成为并集的类型

```ts
PermutationsOfTuple<[1, number, unknown]>;
// Should return:
// | [1, number, unknown]
// | [1, unknown, number]
// | [number, 1, unknown]
// | [unknown, 1, number]
// | [number, unknown, 1]
// | [unknown, number ,1]
```

思路：

- 首先实现一个插入方法，可以将类型插入到元组的任意两个元素中间
- 如果输入的参数是联合类型的元组，则会触发分布式，会返回所有组合

```ts
type Insert<T extends any[], U> = T extends [infer L, ...infer R] ? [L, U, ...R] | [L, ...Insert<R, U>] : [];
```

- 用一个空的元组来保存最终结果（实际上是元组的联合类型）
- 遍历参数元组，依次把每一个元素通过插入方法插入到保存元组中

```ts
type PermutationsOfTuple<T extends unknow[], U extends any[] = []> = T extends [infer L, ...infer R]
  ? PermutationsOfTuple<R, Insert<U, L> | [L, ...U]>
  : U;
```
