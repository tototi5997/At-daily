[实现 Triangular](https://github.com/type-challenges/type-challenges/blob/main/questions/27152-medium-triangular-number/README.md)

实现类型 `Triangular<N>` ，给定一个数字 N，找到第 N 个三角形数，即 1 + 2 + 3 + ... + N

思路： 动态规划

`Triangular<N> = Triangular<N - 1> + N`

首先需要实现一个减 1 的减去操作：`SubtractOne<N>`

然后实现一个相加的操作：`Sum<T, N>`

```ts
type SubtractOne<T extends number, A extends any[] = []> = T extends 0
  ? 0
  : [...A, 1]["length"] extends T
  ? A["length"]
  : SubtractOne<T, [...A, 1]>;
```

```ts
type Sum<T extends number, N extends number, A extends any[] = [], B extends any[] = []> = A["length"] extends T
  ? B["length"] extends N
    ? [...A, ...B]["length"]
    : Sum<T, N, A, [...B, 1]>
  : Sum<T, N, [...A, 1], B>;
```

```ts
type Triangular<N extends number> = N extends 0 ? 0 : Sum<Triangular<SubtractOne<N>>, N>;
```

由于我们的 Sum 和 SubtractOne 方法是通过扩充数组来实现的，所以对于超过 1000 的值，将不能正常工作

其实我们的思路比较复杂了，可以换一种思路

可以用两个辅助数组来处理，数组 A 用来判断是否达到 N 个长度，如果没有达到，就自己新增一个元素（+1）

数组 B 用来累计总和，将每次 A 的长度累加进去，最后当 A 长度达到的时候，返回 B 的长度即可

```ts
type Triangular<N extends number, A extends any[] = [], B extends any[] = []> = A["length"] extends N
  ? B["length"]
  : Triangular<N, [...A, 1], [...B, ...A, 1]>;
```
