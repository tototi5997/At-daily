[实现 ExtractToObject](https://github.com/type-challenges/type-challenges/blob/main/questions/29650-medium-extracttoobject/README.md)

实现一个将 prop 值提取到接口的类型。该类型采用两个参数。输出应该是具有 prop 值的对象。 Prop 值是对象

```ts
type Test = { id: "1"; myProp: { foo: "2" } };
type Result = ExtractToObject<Test, "myProp">; // expected to be { id: '1', foo: '2' }
```

遍历对象类型的 key, 对传入的键名做过滤处理

取出对应的对象之后，使用 & 链接两个对象，并使用 omit 来合并

```ts
type ExtractToObject<T extends Record<string, unknown>, U extends keyof T> = Omit<
  {
    [key in keyof T as key extends U ? never : key]: T[key];
  } & {
    [key in keyof T[U]]: T[U][key];
  },
  ""
>;
```
