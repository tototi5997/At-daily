[实现 PublicType](https://github.com/type-challenges/type-challenges/blob/main/questions/28333-medium-public-type/README.md)

从给定类型 T 中删除以 `_` 开头的键

```ts
type s1 = PublicType<{ a: number; _b: string }>; // { a: number }
type s2 = PublicType<{ _n: any[] }>; // {}
```

遍历每一个键名，判断是否是 `_` 开头，过滤掉符合条件的键名

```ts
type PublicType<T extends object> = {
  [key in keyof T as key extends `_${any}` ? never : key]: T[key];
};
```
