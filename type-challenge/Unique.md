[实现 Unique](https://github.com/type-challenges/type-challenges/blob/main/questions/05360-medium-unique/README.zh-CN.md)

实现类型版本的 Lodash.uniq 方法, Unique 接收数组类型 T, 返回去重后的数组类型.

```ts
type Res = Unique<[1, 1, 2, 2, 3, 3]>; // [1, 2, 3]
type Res1 = Unique<[1, 2, 3, 4, 4, 5, 6, 7]>; // [1, 2, 3, 4, 5, 6, 7]
```

遍历元组，用一个额外的空数组来统计遍历的元素

如果元素存在于统计元组中，则跳过，继续遍历下一个元素

遍历结束返回统计元组

难点在于，如何判断元素是否存在于统计元组中

如果用 `extends` 来判断，无法判断 string 类型和 文本类型

可以使用 Equal 函数来判断两个类型是否相等

```ts
type Equal<X, Y> = (<T>() => T extends X ? 1 : 2) extends <T>() => T extends Y ? 1 : 2 ? true : false;
```

PS:为了区分 unknow, any 和 never 类型，通常我们用 Equal 来区分，如果单纯使用双向 extends,通常 any 和 unkow 是无法区分的

在实现一个类型 Includes 判断元素是否在元组中

```ts
type Includes<T extedns any[], U> = T extends [infer L, ...infer R] ? Equal<L, U> ? true : Includes<R, U> : false
```

```ts
type Unique<T extends any[], U extends any[] = []> = T extends [infer L, ...infer R]
  ? Includes<U, L> extends true
    ? Unique<R, U>
    : Unique<R, [...U, L]>
  : U;
```
