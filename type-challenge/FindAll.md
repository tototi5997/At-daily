[实现 FindAll](https://github.com/type-challenges/type-challenges/blob/main/questions/21104-medium-findall/README.md)

`FindAll<T extends string, P extends string>` 给定模式字符串 P 和文本字符串 T，实现 FindAll<T, P> 类型，该类型返回一个数组，其中包含 T 中 P 匹配的所有索引（从 0 开始索引）。

```ts
type a1 = FindAll<"AAAA", "A">; // [0, 1, 2, 3]
type a2 = FindAll<"Collection of TypeScript type challenges", "pe">; // [16, 27]
```

思路：

- 如果第二个参数是空字符，返回 []
- 用一个辅助数组来保存当前索引，一个辅助数组用来保存模板模板出现的索引
- 遍历字符串，判断当前字符串是否是以目标模板作为开始
  - 如果是，保存，继续遍历后面的字符
  - 如果不是，继续遍历后面的字符
- 遍历完毕，返回保存模板的辅助数组

```ts
type FindAll<T extends string, P extends string, C extends any[] = [], U extends any[] = []> = P extends ""
  ? []
  : T extends `${string}${infer R}`
  ? T extends `${P}${string}`
    ? FindAll<R, P, [...C, 1], [...U, C["length"]]>
    : FindAll<R, P, [...C, 1], U>
  : U;
```

给你一张 无向 图，图中有 n 个节点，节点编号从 0 到 n - 1 （都包括）。同时给你一个下标从 0 开始的整数数组 values ，其中 values[i] 是第 i 个节点的 价值 。同时给你一个下标从 0 开始的二维整数数组 edges ，其中 edges[j] = [uj, vj, timej] 表示节点 uj 和 vj 之间有一条需要 timej 秒才能通过的无向边。最后，给你一个整数 maxTime 。

合法路径 指的是图中任意一条从节点 0 开始，最终回到节点 0 ，且花费的总时间 不超过 maxTime 秒的一条路径。你可以访问一个节点任意次。一条合法路径的 价值 定义为路径中 不同节点 的价值 之和 （每个节点的价值 至多 算入价值总和中一次）。

请你返回一条合法路径的 最大 价值。

仔细阅读题目描述我们可以发现，timej 的最小值为 10，而 maxTime 的最大值为 100。这说明我们至多只会经过图上的 10 条边。由于图中每个节点的度数都不超过 4，因此我们可以枚举所有从节点 0 开始的路径。

我们可以使用递归 + 回溯的方法进行枚举。递归函数记录当前所在的节点编号，已经过的路径的总时间以及节点的价值之和。如果当前在节点 u，我们可以枚举与 u 直接相连的节点 v 进行递归搜索。在搜索的过程中，如果我们回到了节点 0，就可以对答案进行更新；如果总时间超过了 maxTime，我们需要停止搜索，进行回溯。
