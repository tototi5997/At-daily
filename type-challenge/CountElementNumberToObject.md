[实现 CountElementNumberToObject](https://github.com/type-challenges/type-challenges/blob/main/questions/09989-medium-tong-ji-shu-zu-zhong-de-yuan-su-ge-shu/README.zh-CN.md)

实现 CountElementNumberToObject<T>, 通过实现一个 CountElementNumberToObject 方法，统计数组中相同元素的个数

```ts
type Simple1 = CountElementNumberToObject<[1, 2, 3, 4, 5]>; // {1: 1, 2: 1, 3: 1, 4: 1, 5: 1}
type Simple2 = CountElementNumberToObject<[1, [1, 2]]>; // {1 : 2, 2: 1}
```

思路：

- 传入的元组可能存在嵌套，第一步需要把元组扁平化处理
- 对元组扁平化之后，对元组中元素个数进行统计
- 实现两个工具类型即可

扁平化元组

```ts
type Flatten<T extends any[], U extends any[] = []> = T extends [infer L, ...infer R]
  ? L extends any[]
    ? Flatten<R, [...U, ...Flatten<L>]>
    : Flatten<R, [...U, L]>
  : U;
```

统计元组中某个元素的个数

```ts
type GetCount<T extends any[], N extends any, C extends any[] = []> = T extends [infer L, ...infer R]
  ? Equal<L, N> extends true
    ? GetCount<R, N, [...C, 1]>
    : GetCount<R, N, C>
  : C["length"];
```

```ts
type CountElementNumberToObject<T extends any[]> = {
  [key in Flatten<T>[number]]: GetCount<Flatten<T>, key>;
};
```
