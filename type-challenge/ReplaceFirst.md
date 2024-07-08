[实现 ReplaceFirst](https://github.com/type-challenges/type-challenges/blob/main/questions/25170-medium-replace-first/README.md)

实现类型 ReplaceFirst<T, S, R> ，它将用 R 替换元组 T 中第一次出现的 S。如果 T 中不存在这样的 S，则结果应为 T。

```ts
type s1 = ReplaceFirst<[1, 2, 3], 3, 4>; // [1,2,4]
type s2 = ReplaceFirst<["A", "B", "C"], "C", "D">; // ['A, 'B', 'D']
```

思路：

- 新增一个辅助元组用来保存遍历过的元素
- 遍历元组，判断当前元素是否 extends S
  - 如果可以，替换当前元素为 R，直接返回修改过的元组
  - 如果不可以，继续遍历，直到完成遍历
- 如果没有找到目标元素，返回辅助元组

```ts
type ReplaceFirst<T extends readonly unknown[], S, U, C extends any[] = []> = T extends [infer L, ...infer R]
  ? L extends S
    ? [...C, U, ...R]
    : ReplaceFirst<R, S, U, [...C, L]>
  : C;
```