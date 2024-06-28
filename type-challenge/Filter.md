[实现 Filter](https://github.com/type-challenges/type-challenges/blob/main/questions/18220-medium-filter/README.md)

实现类型 Filter<T, Predicate> 接受数组 T、原始类型或联合原始类型 Predicate，并返回包含 Predicate 元素的数组

```ts
type Falsy = false | 0 | "" | null | undefined;
type preview = Filter<[0, 1, 2], Falsy>; // [0]
```

思路：

- 遍历元素，查看是否可以 extends 第二个参数的类型
- 如果可以，保存，遍历下一个
- 如果不可以，遍历下一个
- 遍历结束，返回结果

```ts
type Filter<T, U, C extends any[] = []> = T extends [infer L, ...infer R] ? (L extends U ? Filter<R, U, [...C, L]> : Filter<R, U, C>) : C;
```


```ts
type GetFilter<T, U> = T extends [infer L, ...infer R] ? L extends U
```