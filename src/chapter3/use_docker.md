# docker imageを使う

Dockerイメージが多くの人によって作成され、公開されています。
ここでは@amutakeによって作成されている[docker-satysfi](https://github.com/amutake/docker-satysfi)を紹介します。

## Dockerのインストール

まずはDockerをインストールします。
Dockerのインストール方法については[Dockerの公式ドキュメント](https://docs.docker.com/get-docker/)をご覧ください。

インストールが完了したら以下のコマンドを実行し、`Hello from Docker!`と表示されることを確認してください。

```sh
docker run hello-world
```

## docker-satysfiの基本的な使い方

docker-satysfiは[`amutake/satysfi`](https://hub.docker.com/r/amutake/satysfi)という名前でDocker Hubから提供されています。
このDockerイメージには`satysfi`コマンドと`satyrographos`コマンドが含まれており、`docker run amutake/satysfi`以降に指定することでこれらのコマンドを利用することができます。

例えば`satysfi`や`satyrographos`のバージョンを確認したい場合は以下のようにします。

```sh
docker run amutake/satysfi satysfi --version
docker run amutake/satysfi satyrographos version
```

### 文書ファイルをコンパイルする

ではdocker-satysfiに含まれる`satysfi`コマンドを使って文書ファイルをコンパイルしてみましょう。なお、`satysfi`コマンドの詳しい使い方は後述の[Hello SATySFi!](./hello_satysfi.md)に書かれているので、ここでは`docker`コマンドのオプションのみ説明します。

カレントディレクトリに`demo.saty`という文書ファイルがあり、この文書ファイルをコンパイルしたい場合、以下のコマンドを実行します。

```sh
docker run --rm -v $(pwd):/satysfi amutake/satysfi satysfi demo.saty
```

これで`demo.pdf`というファイルができあがります。

このコマンドを分解してみると以下のようになります。

- `docker run`: Dockerコンテナを実行
- `--rm`: 終了時にコンテナを削除（省略可）
- `-v $(pwd):/satysfi`: カレントディレクトリのファイルを`/satysfi`にマウント
- `amutake/satysfi`: イメージ名
- `satysfi demo.saty`: `demo.saty`をコンパイル

docker-satysfiでは`/satysfi`というディレクトリがデフォルトのワーキングディレクトリとして設定されています。
コンテナ内で実行されるコマンドはこのディレクトリ内で実行されます。

この例ではホストのカレントディレクトリに`demo.saty`があるので、`-v $(pwd):/satysfi`を指定することでコンテナ内から`demo.saty`が見えるようになり、`satysfi demo.saty`でコンパイルすることができるようになります。

コンパイル結果のPDFはコンテナ内の`/satysfi`に出力されます。`/satysfi`にはホストのカレントディレクトリがマウントされているので、結果としてPDFはホストのカレントディレクトリに置かれるということになります。

### SATySFiのバージョンを指定する

SATySFiの特定のバージョンを使いたい場合はdocker-satysfiを使うと簡単です。
例えば`v0.0.6`を使いたい場合は、以下のように`amutake/satysfi:0.0.6`と指定します。

```sh
docker run --rm amutake/satysfi:0.0.6 satysfi --version
```

上記のコマンドを実行すると`SATySFi version 0.0.6`と表示され、確かに`v0.0.6`が使われていることが確認できます。

## docker-satysfiの発展的な使い方

### opamとsatyrographosでライブラリをインストールする

docker-satysfiには（後述する一部のタグを除いて）`opam`コマンドと`satyrographos`コマンドが含まれているため、`satyrographos-repo`に登録されているSATySFiのライブラリを利用することができます。

例えば[`satysfi-base`](https://github.com/nyuichi/satysfi-base/)をインストールしつつ`demo.saty`という文書ファイルをコンパイルしたい場合、以下のようにします。

```sh
docker run --rm -v $(pwd):/satysfi amutake/satysfi \
  sh -c "opam update && opam install satysfi-base && satyrographos install && satysfi demo.saty"
```

Satyrographosの詳しい使い方については[パッケージの読み込み・インストール](../chapter4/import_package.md)を参照してください。

### より小さいサイズのイメージを使う

ここまでの説明で使ってきたイメージは圧縮後で1GB程度という比較的大きいイメージでした。
これは、OCamlコンパイラや、SATySFi・Satyrographos自体のビルドに必要なライブラリがすべて含まれているためです。

docker-satysfiでは、CIなどでの利用でイメージのダウンロード時間を少しでも削減したい場合や、機能を削ってもいいので小さいサイズのイメージが使いたい場合のために、`slim`タグと`opam-slim`タグが用意されています。

`slim`タグがつけられているイメージは`satysfi`コマンドと`satyrographos`コマンドのみを含むイメージです。
docker-satysfiでは最も小さいイメージですが、`opam`やOCamlコンパイラは含まれていません。

`opam-slim`タグのイメージには`satysfi`コマンドや`satyrographos`コマンドそして`opam`コマンドが含まれていますが、OCamlコンパイラなど、`satysfi`や`satyrographos`をビルドするのに必要なライブラリは一切含まれていません。しかし通常SATySFiで文書をコンパイルする場合はこれらのOCamlライブラリは必要ないことが多いので、ほとんどのユースケースはこのイメージでカバーできます。なお、2021-03-24時点で`opam-slim`は実験的なイメージであることに注意してください。

以上のように、`slim`や`opam-slim`は機能を削っている代わりに小さいイメージとなっています。

なお、`slim`と`opam-slim`はともに`0.0.6-slim`や`0.0.6-opam-slim`と指定することでSATySFiの特定のバージョンで利用することができます。

### SATySFiの最新版を使う

docker-satysfiでは`nightly`というタグも提供しています。
これは毎日日本時間9:00にその時点でのSATySFiとSatyrographosの最新版をビルドしているイメージです。

`nightly`タグは`slim`タグと同様に`satysfi`と`satyrographos`しか含まれていない（`opam`コマンドが含まれていない）ことに注意してください。
