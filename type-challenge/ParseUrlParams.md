[实现 ParseUrlParams](https://github.com/type-challenges/type-challenges/blob/main/questions/09616-medium-parse-url-params/README.md)

实现一个类型级解析器来将 URL 参数字符串解析为 Union

```ts
ParseUrlParams<":id">; // id
ParseUrlParams<"posts/:id">; // id
ParseUrlParams<"posts/:id/:user">; // id | user
```

解法 1:

思路：

- 使用 / 分割字符串，获得参数数组
- 判断数组中的每个元素是否是 :param 参数，如果是，保存起来
- 数组转联合类型

```ts
type Split<T> = T extends `${infer L}/${infer R}` ? [L, ...Split<R>] : [T];
type ParseArr<T extends any[]> = T extends [infer L, ...infer R] ? (L extends `:${infer U}` ? [U, ...ParseArr<R>] : [...ParseArr<R>]) : [];
type ParseUrlParams<T> = ParseArr<Split<T>>[number];
```

解法 2:

思路：

- 思路同 1，直接原地处理字符串，通过 : 优先处理字符串

```ts
type ParseUrlParams<T> = T extends `${string}:${infer R}` ? (R extends `${infer L}/${infer E}` ? L | ParseUrlParams<E> : R) : never;
```
