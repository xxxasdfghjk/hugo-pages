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
type MyExclude<T, U> = T extends U ? never : T;
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

### 189・Awaited

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00189-easy-awaited/README.md)

`Promise<T>`を unwrap して`T`を取り出す`Awaited<U>`を実装する問題。

#### 解答

```ts
type MyAwaited<T> = T extends { then: (onfulfiled: (arg: infer U) => any) => any } ? MyAwaited<U> : T;
```

#### メモ

想定解がよくわからない。

再帰的に`MyAwaited<U>`を適用し、`Promise<T>`にマッチしなくなったときの型を返す。

※追記
JavaScriptだとthenメソッドを持つオブジェクトはPromiseと判定されるらしい。TypeScriptでは、`PromiseLike<T>`なる型でそれを表現できる。それを使って答えると次のようになる。

```ts
type MyAwaited<T> = T extends PromiseLike<infer U> ? MyAwaited<U> : T
```

### 268・If

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00268-easy-if/README.md)

`If<C,T,F>` を実装する。
`C`は`true|false`リテラル型であり、`true`のとき`T`、`false`のとき`F`型を返す。

#### 解答

```ts
type If<C extends true|false, T, F> = C extends true ? T : F
```

#### メモ
特になし

### 533・Concat
[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00533-easy-concat/README.md)

2つのタプルをとり、連結した1つのタプルを返す `Concat<T1,T2>`を実装する。

#### 解答
```ts
type Concat<T extends readonly any[], U extends readonly any[]> = [...T,...U]
```

#### メモ
型の文脈でもspread構文は存在するらしい。

### 898・includes
[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/00898-easy-includes/README.md)

タプルに特定の型が存在するか判定する`Includes<T,U>`を実装する。

#### 解答
```ts
type Includes<T extends readonly any[], U> =  T extends [infer V , ...infer Rest] ? (Equal<V,U> extends true ? true : Includes<Rest,U>) : false

```

#### メモ

`Equal<T,U>`を使うのは盲点。それがわかれば再帰で実装するのみ。

### 3057・Push

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/03057-easy-push/README.md)

タプルに型を1つ追加する`Push<T,U>`を実装する。

#### 解答
```ts
type Push<T extends readonly any[], U> = [...T, ...[U]]
```

#### メモ
特になし。

### 3312・Parameters

[問題はこちら](https://github.com/type-challenges/type-challenges/blob/main/questions/03312-easy-parameters/README.md)

関数を受け取り引数のタプルを返す`Parameters<F>`を実装する。

#### 解答
```ts
type MyParameters<T extends (...args: any[]) => any> = T extends (...args: infer U) => any ? U : never
```

#### メモ
引数をスプレッド構文で引き受ける典型。

