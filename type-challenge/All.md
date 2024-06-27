[实现 All](https://github.com/type-challenges/type-challenges/blob/main/questions/18142-medium-all/README.md)

实现类型 `All<T>` ，如果列表中的所有元素都等于传入的第二个参数，则返回 true，如果存在任何不匹配，则返回 false

```ts
type Test1 = [1, 1, 1];
type Test2 = [1, 1, 2];

type Todo = All<Test1, 1>; // should be same as true
type Todo2 = All<Test2, 1>; // should be same as false
```

思路：

- 元组类型转联合类型，判断是否能 extends 第二个参数

```ts
type All<T extends any[], U> = T[number] extends U ? true : false;
```

- `All<[any], unknown>` 这种情况，返回的 false
- 使用 `Equal<T, R>` 来判断两种类型

```ts
type All<T extends any[], U> = Equal<T[number], U>;
```

- `All<[1, 1, 2], 1 | 2>` 元组转换成的联合类型刚好是第二个参数，则无法判断
- 只能依次遍历，一个一个判断

```ts
type All<T extends any[], U> = T extends [infer L, ...infer R] ? (Equal<L, U> extends true ? All<R, U> : false) : true;
```
