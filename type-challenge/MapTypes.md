[实现 MapTypes](https://github.com/type-challenges/type-challenges/blob/main/questions/05821-medium-maptypes/README.md)

实现 `MapTypes<T, R>` ，它将对象 T 中的类型转换为类型 R 定义的不同类型

```ts
type StringToNumber = {
  mapFrom: string; // value of key which value is string
  mapTo: number; // will be transformed for number
};

type StringToNumber = { mapFrom: string; mapTo: number };
MapTypes<{ iWillBeANumberOneDay: string }, StringToNumber>; // gives { iWillBeANumberOneDay: number; }
```

思路：

- 遍历属性值， 判断属性值是否匹配 mapFrom 对应的类型
- 如果相匹配，替换成 mapTo 的类型
- 如果第二个参数类型为联合类型，需要对联合类型进行遍历处理

```ts
type MapType<T, R extends { mapFrom: any; mapTo: any }> = {
  [key in keyof T]: T[key] extends R["mapFrom"] ? (R extends { mapFrom: T[key] } ? R["mapFrom"] : never) : T[key];
};
```

PS: extends 在这里的作用表示是否可以兼容，因此 `{ mapFrom: stirng, mapTo: number } extends { mapFrom: string} ` 为 true
