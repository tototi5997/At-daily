[实现 Pick](https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.zh-CN.md)

从类型 `T` 中选出符合 `K` 的属性，构造一个新的类型

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = MyPick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

思路：

- 遍历联合类型 R,返回存在的 T[key]

```ts
type Pick<T, R extends keyof T> = {
  [key in R]: T[key];
};
```
