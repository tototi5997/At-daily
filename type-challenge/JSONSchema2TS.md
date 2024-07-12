[实现 JSONSchema2TS](https://github.com/type-challenges/type-challenges/blob/main/questions/26401-medium-json-schema-to-typescript/README.md)

实现通用类型 JSONSchema2TS，它将返回与给定 JSON 模式相对应的 TypeScript 类型

```ts
type Type14 = JSONSchema2TS<{
  type: "object";
  properties: {
    req1: { type: "string" };
    req2: {
      type: "object";
      properties: {
        a: {
          type: "number";
        };
      };
      required: ["a"];
    };
    add1: { type: "string" };
    add2: {
      type: "array";
      items: {
        type: "number";
      };
    };
  };
  required: ["req1", "req2"];
}>;

/*
{
  req1: string
  req2: { a: number }
  add1?: string
  add2?: number[]
}
*/
```

在给出的数据结构中，存在 string, number, boolean, enum, object, array 这几种数据类型

其中 string, number, boolean 我们可以直接处理，构造一个 KeyMap，用对应的字符串作为他们的 key

```ts
type KeyMap = {
  string: string;
  boolean: boolean;
  number: number;
};
```

我们发现，对于复杂的数据类型，enum 有额外的 enum 属性，object 有额外的 properties 和 required(可选) 属性，array 有额外的 items 属性

因此，我们可以优先判断 object 和 array, 最后判断 enum 和基本属性，其中 enum 和 基本属性可以一起判断

HandleObject<T> -- 处理 object 结构
HandleArray<T> -- 处理 array 结构
HandleEnumOrBasic<T> -- 处理 enum 和基本数据结构

因此，整个判断逻辑是这样的：

```ts
type JSONSchema2TS<T> = T extends { type: infer Type }
  ? Type extends "array"
    ? HandleArray<T>
    : Type extends "object"
    ? HandleObject<T>
    : HandleEnum<T, Type & keyof KeyMap>
  : never;
```

在判断 enum 类型的时候，因为 Type 类型无法确认是不是 KeyMap 的 key，因此需要做一个并集规避 ts 的报错

HandleEnumOrBasic 实现如下：

```ts
type HandleEnumOrBasic<T, Type extends keyof KeyMap> = T extends { enum: infer Enum extends any[] } ? Enum[number] : KeyMap[Type];
```

HandleArray 实现如下：

```ts
type HandleArray<T> = T extends { items: infer Item } ? JSONSchema2TS<Item>[] : unknown[];
```

HandleObject 实现如下：

```ts
type HandleObject<T> = T extends { properties: infer Prop extends Record<string, any> }
  ? T extends { required: infer Required extends string[] }
    ? Omit<
        { [key in Required[number]]: JSONSchema2TS<Prop[key]> } & {
          [key in Exclude<keyof Prop, Required[number]>]?: JSONSchema2TS<Prop[key]>;
        },
        never
      >
    : { [key in keyof Prop]?: JSONSchema2TS<Prop[key]> }
  : Record<string, unknown>;
```

object 的 key 需要对 required 进行区分，我们使用 & 来合并两个对象，并用 Omit 进行处理最终的结果