---
title: "wordleソルバを作りました"
date: 2022-10-20T00:45:32+09:00
draft: false
---

ちょっと旬は過ぎた感はありますがTypeScriptの練習がてらwordleソルバを作りました。

https://github.com/xxxasdfghjk/wordle-solve-js

<!--more-->
また、インタラクティブに最適解を調べることができるようにアプリを作りました。

gh-pagesはreactアプリを公開できるので楽しいですね

https://xxxasdfghjk.github.io/wordle-solve-js/

アルゴリズムはエントロピー利得平均情報量の最大化を使いました。ネットにごろごろ転がっているネタではありますが... 

大体平均すると3.5手強で回答可能で、wordleの回答になりうるワードはすべて6手以内に回答できます。

なにやらWordleソルバの性能を競うサイトがあるらしく、何か思いついたらここに投稿してみるのもおもしろそうです。

https://freshman.dev/wordle/leaderboard