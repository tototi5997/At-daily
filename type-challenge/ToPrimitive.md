[实现 ToPrimitive](https://github.com/type-challenges/type-challenges/blob/main/questions/16259-medium-to-primitive/README.md)

实现类型 ToPrimitive<T> , 将文字类型（标签类型）的属性转换为原始类型。

```ts
type X = {
  name: "Tom";
  age: 30;
  married: false;
  addr: {
    home: "123456";
    phone: "13111111111";
  };
};

type Expected = {
  name: string;
  age: number;
  married: boolean;
  addr: {
    home: string;
    phone: string;
  };
};
type Todo = ToPrimitive<X>; // should be same as `Expected`
```

思路：

- 首先需要了解如何获取文字类型的原始类型
  - `T extends { valueOf: () => infer P } ? P : T`，通过 valueOf 这个方法，可以返回原始类型
- 对输入类型进行判断，是否是 object 类型
  - 不是，直接获取原始类型
  - 是，判断是 function 还是普通的 object
- 如果是普通的 object, 遍历每一项元素，递归获取每一项元素的类型

```ts
type ToPrimitive<T> = T extends object
  ? T extends (...args: any[]) => any
    ? Function
    : {
        [k in keyof T]: ToPrimitive<T[k]>;
      }
  : T extends { valueOf: () => infer P }
  ? P
  : T;
```
