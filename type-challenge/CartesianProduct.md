[实现 CartesianProduct](https://github.com/type-challenges/type-challenges/blob/main/questions/27862-medium-cartesianproduct/README.md)

实现 `CartesianProduct<T, U>` , 给定 2 个集合（并集），返回一组元组中的笛卡尔积

```ts
type s1 = CartesianProduct<1 | 2, "a" | "b">; // [1, 'a'] | [2, 'a'] | [1, 'b'] | [2, 'b']
```

看到联合类型的互相组合，想到应该使用联合类型的分布式特性

首先触发 T 的分布式，同时触发 U 的分布式，组合当前的 `[T, U]`

```ts
type CartesianProduct<T, U, T2 = T> = T extends T ? (U extends U ? [T, U] : never) : never;
```
