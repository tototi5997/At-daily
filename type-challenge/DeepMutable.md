[实现 DeepMutable](https://github.com/type-challenges/type-challenges/blob/main/questions/17973-medium-deepmutable/README.md)

实现一个通用的 DeepMutable，它使对象及其子对象的每个参数递归地可变。

```ts
type X = {
  readonly a: () => 1;
  readonly b: string;
  readonly c: {
    readonly d: boolean;
    readonly e: {
      readonly g: {
        readonly h: {
          readonly i: true;
          readonly j: "s";
        };
        readonly k: "hello";
      };
    };
  };
};

type Expected = {
  a: () => 1;
  b: string;
  c: {
    d: boolean;
    e: {
      g: {
        h: {
          i: true;
          j: "s";
        };
        k: "hello";
      };
    };
  };
};

type Todo = DeepMutable<X>; // should be same as `Expected`
```

思路：

- 只读属性变为可读属性 `- readonly key`
- 对对象类型进行遍历，去除只读属性
- 属性值如果是 Function 类型，直接返回
- 如果是对象类型，需要递归遍历属性值
- 普通类型直接返回

```ts
type DeepMutable<T extends Record<string, any>> = T extends Function
  ? T
  : {
      -readonly [k in keyof T]: DeepMutable<T[k]>;
    };
```
