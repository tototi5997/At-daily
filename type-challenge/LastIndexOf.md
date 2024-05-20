[实现 LastIndexOf](https://github.com/type-challenges/type-challenges/blob/main/questions/05317-medium-lastindexof/README.md)

实现类型版本的 `Array.lastIndexOf`, `LastIndexOf<T, U>` 接受数组 T, any 类型 U, 如果 U 存在于 T 中, 返回 U 在数组 T 中最后一个位置的索引, 不存在则返回 -1

```typescript
type Res1 = LastIndexOf<[1, 2, 3, 2, 1], 2>; // 3
type Res2 = LastIndexOf<[0, 0, 0], 2>; // -1
```

思路：

- 从元组的最后一位向前开始遍历
- 当找到第一个元素时，剩余元素的长度就是最后位的索引号

```ts
type AllEqual<T, U> = T extends U ? (U extends T ? true : false) : false;
type LastIndexOf<T, U> = T extends [...infer L, infer R] ? (AllEqual<R, U> extends true ? L["length"] : LastIndexOf<L, U>) : -1;
```
