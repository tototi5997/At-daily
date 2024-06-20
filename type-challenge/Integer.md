[实现 Integer](https://github.com/type-challenges/type-challenges/blob/main/questions/10969-medium-integer/README.md)

实现类型 `Integer<T>` 类型 T 继承自 number，如果 T 是整数则返回它，否则返回 never

```ts
type a = Integer<1>; // true
type b = Integer<1.2>; // false
```

思路：

- 可以将类型转换次字符串
- 判断字符串类型是否符合 `${infer string}.${infer string}`
  - 如果符合说明是浮点类型
  - 不符合说明是整数类型
- 需要对 number 类型进行特殊化处理

```ts
type Integer<T extends number> = number extends T ? never : `${T}` extends `${infer string}.${infer string}` ? never : T;
```

还可以使用 bigint 这个特殊的类型，可以表示任意精度的整数

```ts
type Integer<T extends number> = `${T}` extends `${bigint}` ? T : never;
```
