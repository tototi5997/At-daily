[实现 Combs](https://github.com/type-challenges/type-challenges/blob/main/questions/21106-medium-zu-he-jian-lei-xing-combination-key-type/README.md)

实现类型 Combs<T>，组合多个修饰键，但不能出现相同的修饰键组合。 提供的 ModifierKeys 中，前一个值的优先级高于后一个值；即 cmd ctrl 可以，但 ctrl cmd 不允许。

```ts
type ModifierKeys = ["cmd", "ctrl", "opt", "fn"];
type a1 = Combs<ModifierKeys>; // "cmd ctrl" | "cmd opt" | "cmd fn" | "ctrl opt" | "ctrl fn" | "opt fn"
```

思路：

- 常见的元组转联合类型
- 遍历元组元素，每个元素需要和剩余元素一一组合

```ts
type Combs<T extends string[]> = T extends [infer L extends string, ...infer R extends string[]] ? `${L} ${R[number]}` | Combs<R> : never;
```
