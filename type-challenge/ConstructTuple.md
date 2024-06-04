[实现 ConstructTuple](https://github.com/type-challenges/type-challenges/blob/main/questions/07544-medium-construct-tuple/README.zh-CN.md)

构造一个给定长度的元组。

```ts
type result = ConstructTuple<2>; // [unknown, unkonwn]
```

思路：

- 构造一个空元组
- 如果元组长度没有达到目标长度，往里添加 unknow 元素

```ts
type ConstructTuple<L extends number, U extends unknown[] = []> = U["length"] extends L ? U : ConstructTuple<L, [unknown, ...U]>;
```
