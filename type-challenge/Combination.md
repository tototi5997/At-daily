[实现 Combination](https://github.com/type-challenges/type-challenges/blob/main/questions/08767-medium-combination/README.md)

给定一个字符串数组，进行排列和组合。

```ts
// expected to be `"foo" | "bar" | "baz" | "foo bar" | "foo bar baz" | "foo baz" | "foo baz bar" | "bar foo" | "bar foo baz" | "bar baz" | "bar baz foo" | "baz foo" | "baz foo bar" | "baz bar" | "baz bar foo"`
type Keys = Combination<["foo", "bar", "baz"]>;
```

思路：

- 答案返回的是联合类型，因此首先吧元组转联合类型 `T[number]`
- 当得到一种类型的时候，需要考虑，当前类型和剩余类型自由组合成新类型
- 比如 `foo` 和 剩余类型 `[bar, baz]`, `bar` 和 `[foo, baz]`
- 将类型转还成联合类型后，还需要对联合类型进行遍历，可以使用分布式来实现

```ts
type Combination<T extends string[], U extends stirng = T[number], A extends string = U> = U extends A
  ? U | `${U} ${Combination<T, Exclude<U, A>>}`
  : never;
```
