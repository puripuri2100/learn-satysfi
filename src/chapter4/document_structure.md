# 文書構造の記述

章節を記述することができます。
構造的な文章を書くためには欠かせません。

stdjaとstdjabookの両パッケージでは`+section`と`+subsection`コマンドが、stdjareportパッケージでは`+chapter`・`+section`・`+subsection`の3つのコマンドが提供されます。

どのコマンドも`+section{（タイトル）}<（中身）>`という使い方をします。

例えば

```
+section {1節} <
  +subsection{小節} <
    +p{段落}
  >
  +subsection{小節その2} <
    +p{段落2}
  >
>
```

のような感じです。
