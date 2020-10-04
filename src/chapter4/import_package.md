# パッケージの読み込み

`@require: （パッケージ名）`や`@import: （パッケージファイルへの相対パス）`でパッケージの読み込みができます。
パッケージを読み込む場合は、拡張子を必ず省略します。

# パッケージのインストール

satyrographosを使うことで有志の作成したパッケージを簡単にインストールすることができます。

例えば、[satysfi-base](https://github.com/nyuichi/satysfi-base)というパッケージ群をインストールするには

```
opam update
opam install satysfi-base
satyrographos install
```

をするだけです。

ここでインストールしたパッケージは`@require: base/int`のようにして読み込むことができます。
satyrographosによってインストールされるパッケージは、ファイル名の衝突を避けるためにライブラリごとに一つのフォルダに入っています。そのため、そのフォルダを含めて指定する必要があります。

satyrographosによってインストールされるライブラリの名前は`satysfi-`から始まるようになっています。
しかし、satyrographosによってシステムにインストールされるときには基本的に`satysfi-`の部分が外れるようになっています。詳しくは[satyrographosのREADME](https://github.com/na4zagin3/satyrographos#readme)や「[Satyrographos でパッケージの簡単インストール](https://qiita.com/na4zagin3/items/14fe2647b663eeac6ac2)」を読んでください。

インストールできるライブラリの一覧は

```
opam list 'satysfi-*'
```

によって確認できます。

# 手動でパッケージをインストールする

satyrographosを使っている場合、`~/.satysfi/dist/packages/`にパッケージはインストールされます。

しかし、`~/.satysfi/dist/`以下は基本的に人が弄ることは想定されていません。
そこで、ユーザ自身がパッケージ類を格納することを想定している`~/.satysfi/local/`以下にパッケージファイルをコピーします。

distの方が優先順位が高いので、distからの相対パスとlocalからの相対パスが同じファイルが存在しない限り、localに置いたファイルが読み込まれます。
