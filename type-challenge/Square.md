[实现 Square](https://github.com/type-challenges/type-challenges/blob/main/questions/27133-medium-square/README.md)

实现类型 Square<T>，给定一个数字，您的类型应该返回它的平方。

```ts
type s1 = Square<2>; // 4
type s2 = Square<-2>; // 4
```

思路： `len * count`，比如 3^2, 即 3 个 3

但是这个解法只能处理结果为 1000 以下的数值

首先需要实现的是一个取绝对值的方法，让负数和正数得到相同的处理

```ts
type Abs<T extends number> = `${T}` extends `-${infer R extends number}` ? R : T;
```

定义两个辅助数组，一个数组用来添加数值达到 count，另一个数组用来记录 count 数组，直到它的长度也达到 count

最终返回的结果，是第二个数组中，所有子项解构之后的结果，因此我们实现一个解构数组的方法

```ts
type ParseRes<T extends any[][]> = T extends [infer L extends any[], ...infer R extends any[][]] ? [...L, ...ParseRes<R>] : [];
```

最终需要做的，就是不断相辅助数组中填数以满足需求

```ts
type Square<N extends number, L extends any[] = [], Res extends any[] = [], NN = HandleNegative<N>> = Res["length"] extends NN
  ? ParseRes<Res>["length"]
  : L["length"] extends NN
  ? Square<N, [], [...Res, L]>
  : Square<N, [...L, 1], Res>;
```
