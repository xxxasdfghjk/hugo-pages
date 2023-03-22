---
title: "type-challengesを埋める"
date: 2023-03-23T23:45:52+09:00
draft: false
---

勉強がてら[type-challenges](https://github.com/type-challenges/type-challenges)なる TypeScript の問題集をやってみる。

<!--more-->

## warm-up

### 13・Hello World

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00013-warm-hello-world/README.md)

解答方法の verify のための設問。

#### 解答

```ts
type HelloWorld = string;
```

#### メモ

なし

## easy

### 4・Pick

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00004-easy-pick/README.md)

`Pick<T,K>`をそれ自身を使わずに実装する問題。

#### 解答

```ts
type MyPick<T, K extends keyof T> = {
    [key in keyof T as key extends K ? key : never]: T[key];
};
```

#### メモ

`K`は`T`のキーである必要があるため、左辺に制約条件を追加する。

### 7・ReadOnly

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00007-easy-readonly/README.md)

`ReadOnly<T>`をそれ自身を使わずに実装する問題。

#### 解答

```ts
type MyReadonly<T> = {
    readonly [key in keyof T]: T[key];
};
```

#### メモ

特になし。

### 11・Tuple to Object

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00011-easy-tuple-to-object/README.md)

与えられた各タプル要素を key/value とする型`TupleToObject<T>`を実装する問題。

#### 解答

```ts
type TupleToObject<T extends readonly PropertyKey[]> = {
    [t in T[number]]: t;
};
```

#### メモ

`[t in T[number]]:t`までは思いついたが、一つエラーが消えず。

諦めて答えを見ると、制約条件に`PropertyKey[]`が必要だった。

`PropertyKey`は`string|number|symbol`の alias で、`[]`の角カッコ内で書ける型全体を指すらしい。自身を key/value としたオブジェクトにするためには、与えられた要素がプロパティで指定可能な型という条件は確かに必要。

### 14・First of Array

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00014-easy-first/README.md)

タプル型を引数にとり、その先頭要素の型を返す`First<T>`を実装する問題。

#### 解答

```ts
type First<T extends any[]> = T extends [] ? never : T[0];
```

#### メモ

indexed access type を使う。

タプルの大きさが 1 以上の場合は`T[0]`で問題ないが、`[]`が渡されたときのみ`never`を返す必要がある。

### 14・Length of Tuple

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00018-easy-tuple-length/README.md)

タプル型の大きさをリテラル型で返す`Length<T>`を実装する問題。

#### 解答

```ts
type Length<T extends readonly any[]> = T["length"];
```

#### メモ

`T["length"]`でタプルの大きさのリテラル型を返すらしい。知らなかった。(ここで知った)[https://www.forcia.com/blog/002377.html]

### 43・Exclude

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00043-easy-exclude/README.md)

`Exclude<T>`を実装する問題。

#### 解答
```ts
type MyExclude<T, U> = T extends U ? never : T
```

#### メモ

ユニオン型`T`を展開した上で評価するのがポイント。
例えば`MyExclude<'a' | 'b' | 'c', 'a' | 'b'>`
が評価されるときは
```ts
'a' extends 'a' | 'b' ? never : 'a'
| 'b' extends 'a' | 'b' ? never : 'b'
| 'c' extends 'a' | 'b' ? never : 'c'
```
と展開され、結果`'c'`になる