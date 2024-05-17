[实现 Pick<T, R>](https://www.typescriptlang.org/play/?#code/PQKgUABBAsELQUHnagG5wgBQJYGMDWl5yFH4BGAnhAIIB2ALgBYD21FAYgK4QAUAAgIZ0AZuwCUEAMSAA70CqyhPbUMzCSXYYANrTgZqYfOP0RAGRmA7t11RAsHKB-eUAUrhAAGmXAB4AKgBoIAaQB8diIBh-lAgXAGUIQFDFQDt-QBC3e0ccVw8fP2jAfKVAX4DAIAYzCBAQQDm5QG8fQGj1exc-QFo5QEgEwC-FQDJvQAgVe08UwD0dQHIDf0AQ80ACBMAAOUAqOUAG02iSvOz8QGj5QCDNQCx-nLtF2gBnfG1aAFMAJ0E+LA3gxgATRggAb3woWgxaNQ2ALghl2i3tAHNLiCON5axXgAdrsxHs9XtQPlAoFhGABbf53TZHR4kRiMO4CfAAXxytDI-wOLmOjDQWw2ADcMBsAO4QAC8EAAsmR4q4iR4AOTXW4bdkQAA+EHZ0LhCI2R3Z3hy0OozwgtCJj0JJxJ5MpNPpF0hkK5d0e7IAwujqBAtqiYey3J8obD4RtEY9dmplhtLVBsVBFnYct4IIAKdQgAHEbvR2CQIIAoOUAp+aAaHdZvRaLR-st7sBgCssPQAHQAK2WmcYWzewGgwAAXvQ4PqAHJgEDAXSgCAAfRbrbbrYggAN5aKAY7lAIAezfbQ6buXrYFx+MZzOwCXcXggGwAHptqEdlhAcBsyIxBMEfRr8ABtTcUbReAC6iuPW-PYGxYEbw6HEEA0raAVejrJNB0+O3WwBg4QLWg5TxA4zggABRABHdg+DUDwIMXfEsGAzEIEEU0YUFHgJw2OAMzgu5wR+YB2GuJ12V0XCICwPhnXXelD3wRDkNoJxoNgtR2KQjYULFABGDwmRZJVGA5HUeW8KTXUgniUPYmC4O41ixQAJiE6dnFE8SbjuXkBSFG1RXFKTvBk1MIBw5Y4CXVibK2U0tnwYSZ1ZE4dO5fTBWFW1ES89ltDJOCMBMy1b3-Ohtl2fZDhOc58AkkEXnefBvl+AEgWoJKwQhGijLtMVkVRI0710dYor2A4WN4xF+Piq5dIeJ5kvBUqIs2HZKtklSjlU+q5Ua7KUutEUCqRCAUTRDYMXvR8fw7QBoOT6QBTa2-ebRwfcAoB9QAwJUAarlu0AY8jABVvOMEyTFM01+LNc3zQti2AARlipbYywrat8B9GMTrOxNk1TdMbrzAsixLZY0TIxQZU+iBABezQAsTRMX6LoB66c2B+7y0rGs-yAA)

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
dynamic programming
