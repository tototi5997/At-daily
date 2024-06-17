[实现 GetMiddleElement](https://github.com/type-challenges/type-challenges/blob/main/questions/09896-medium-get-middle-element/README.zh-CN.md)

通过实现一个 `GetMiddleElement` 方法，获取数组的中间元素，用数组表示

```ts
  type simple1 = GetMiddleElement<[1, 2, 3, 4, 5]>, // 返回 [3]
  type simple2 = GetMiddleElement<[1, 2, 3, 4, 5, 6]> // 返回 [3, 4]
```

思路 1 - 暴力：

- 左右两个空数组，拼凑出目标数组的长度
- 左边长度+右边长度 =？总长度
  - 等于，左右长度是否相等？
    - 如果相等，原数组长度为偶数，返回左边最后一个，右边第一个
    - 不等于，说明奇数个，只需要返回左侧最后一个
  - 不等，向左右两个数组依次添加元素，直到达到目标数组的长度

```ts
type GetMiddleElement<T extends any[], L extends any[] = [], R extends any[] = [], M extends "L" | "R" = "L"> = T["length"] extends 0
  ? []
  : [...L, ...R]["length"] extends T["length"]
  ? L["length"] extends R["length"]
    ? [[1, ...T][L["length"]], [1, ...T][[...L, 1]["length"]]]
    : [[1, ...T][L["length"]]]
  : M extends "L"
  ? GetMiddleElement<T, [...L, 1], R, "R">
  : GetMiddleElement<T, L, [...R, 1], "L">;
```

思路 2：

- 当长度为 0，1，2 的时候，我们发现结果就是原数组
- 可以把整个问题分解成小问题来解决
- 当长度超过 2 的时候，我们分解成判断 `[any, infer M, any]`的形式

```ts
type GetMiddleElement<T extends any[]> = T extends 0 | 1 | 2 ? T : T extends [any, infer M, any] ? GetMiddleElement<M> : [];
```
