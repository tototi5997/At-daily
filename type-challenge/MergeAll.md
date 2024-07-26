[实现 MergeAll](https://github.com/type-challenges/type-challenges/blob/main/questions/27932-medium-mergeall/README.md)

实现类型 MergeAll<T>，将可变数量的类型合并为新类型。如果键重叠，则其值应合并为并集。

思路：

合并两个对象类型，如果同一个键名在两个对象中同时存在，则值为两个值的联合类型

首先找到对象共有的 key，优先构造出联合类型，可以使用 & 来筛选出公共的 key

`[key in keyof T & keyof R]`

剩余的 key 就是各自独特的 key, 直接加入进对象即可

使用 & 求对象的并集，最后使用 Omit 统一成一个对象

```ts
type MergeAll<T, A extends Record<string, unknown> = {}> = T extends [
  infer L extends Record<string, unknown>,
  ...infer R extends Record<string, unknown>[]
]
  ? MergeAll<
      R,
      Omit<
        {
          [key in keyof L & keyof A]: A[key] | L[key];
        } & {
          [key in keyof L as key extends keyof A ? never : key]: L[key];
        } & {
          [key in keyof A as key extends keyof L ? never : key]: A[key];
        },
        never
      >
    >
  : A;
```
