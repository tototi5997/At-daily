[实现 Transpose](https://github.com/type-challenges/type-challenges/blob/main/questions/25270-medium-transpose/README.md)

矩阵的转置是一个将矩阵翻转其对角线的运算符；也就是说，它通过生成另一个矩阵（通常用 AT 表示）来切换矩阵 A 的行索引和列索引。

```ts
type Matrix = Transpose<[[1]]>; // expected to be [[1]]
type Matrix1 = Transpose<[[1, 2], [3, 4]]>; // expected to be [[1, 3], [2, 4]]
type Matrix2 = Transpose<[[1, 2, 3], [4, 5, 6]]>; // expected to be [[1, 4], [2, 5], [3, 6]]
```

思路：

- 我们的目标是，将二维数组的横纵交换
- 那么我们可以这么做，用一个辅助数组来辅助
- 依次遍历数组的每一项，第一次取出第 0 项元素，第二次取出第 1 项元素 ... 第 n 次取出 n - 1 项元素
- 遍历完成返回结果
- 二维数组的长度，就是判断是否遍历完成的依据

```ts
type CollectIndex<T extends number[][], Index extends number> = T extends [infer L extends number[], ...infer R extends number[][]]
  ? [L[Index], ...CollectIndex<R, Index>]
  : [];

type Transpose<T extends number[][], I extends any[] = [], Len extends number = T[0]["length"]> = I["length"] extends Len
  ? []
  : [CollectIndex<T, I["length"]>, ...Transpose<T, [...I, 1]>];
```
