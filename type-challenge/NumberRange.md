[实现 NumberRange](https://github.com/type-challenges/type-challenges/blob/main/questions/08640-medium-number-range/README.md)

实现一个类型限制数字的范围

```ts
type result = NumberRange<2, 9>; //   2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

思路：

- 使用一个计数数组来保存区间值
- 判断是否达到最小值临界点
- 判断是否达到最大值临界点
- 再区间范围内，计数数组的长度就是每次需要插入的数值

- 使用 Flag 来判断是否达到最小值临界点，即 `Flag extends boolean = Count['length'] extends L ? true : false`
- 最大值临界点：`Count['length'] extends H ? end : continue`
- 区间范围内：`[...res, Count['length']]`
- 结果由元组转换成联合类型

```ts
type NumberRange<
  L,
  H,
  Count extends any[] = [],
  R extends any[] = [],
  Flag extends boolean = Count["length"] extends L ? true : false
> = Flag extends true
  ? Count["length"] extends H
    ? [...R, Count["length"]][number]
    : NumberRange<L, H, [...Count, ""], [...R, Count["length"]], Flag>
  : NumberRange<L, H, [...Count, ""]>;
```
