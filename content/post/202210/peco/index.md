---
title: "使っているpecoコマンドまとめ"
date: 2022-10-13T23:26:00+09:00
tags: [blog]
draft: false
summary: "自分がよく使うpecoコマンドをまとめました"
---

### シェル上でリストからのインクリメンタルサーチを可能とするpecoが超便利

lsして、ディレクトリ名を確認して、`cd hogehoge`なんてのは平成で終わりです。

最強のインクリメンタルサーチツール`peco`を使って注意力の消耗とタイポ抽選を回避しましょう。

<!--more-->

使ってるものを適当にまとめ。
何か思いついたら追記するかも。

## lscd
```bash
cd $(ls -d */| peco)
```
とりあえず直下のディレクトリを覗きたいときに。

だけど「やっぱ`cd`したくないのう...」というときはあるもの。
なんとpecoで何も選ばないと引数なし`cd`が実行されてホームディレクトリに送りに...。

関数化して無入力時の処理を書いておくことで回避可能。
```bash
function lscd(){
    local dir="$(ls -1d */ | peco)"
    if [ ! -z "$dir" ] ; then
        cd "$dir"
    fi
}
alias d=lscd
```

## git branch
```bash
git branch -a | grep -v "\->" | peco | sed -e "s/^\*\s*//g"
```

山あるブランチからすぐに選べる。pecoの醍醐味。

`git branch -D`や`git checkout`と組み合わせて使う。

```bash
git checkout -b $(git branch -a | grep -v "\->" | peco | sed -e "s/^\*\s*//g")
```
実際には`git`や`checkout`、この長い後ろの部分は`alias`にしといて、`g o B`で起動するようにしている。

キーに割り当ててもいいかもしれない。

grepオプションはお好みで。

## git log
```bash
git log --pretty=format:"%h %cd %cn:%s" |
 peco --prompt "COMMIT HASH>" | cut -f1 -d " "
```
コミットログからハッシュ値を持ってくるときに使う。

`git rebase`時や`git commit --fixup`のときにめちゃくちゃ便利。

自分は`alias ch='`git log...`'`としておいて、`g rebase -i ch`や`g c --fixup ch`のような感じで使う。