# satyrographosをインストールする

@na4zagin3氏によってsatyrographosというSATySFiのパッケージマネージャが開発されています（日本語読みは「サテュログラポ̣ス」です）。
多数のパッケージがsatyrographosに登録されているばかりか、SATySFi自体もサポートしているため、satyrographosをインストールすることをオススメします。

まだ必要が無いと思う方は先に進み、必要になった時点でこの説明に帰ってくるのが良いでしょう。

satyrographosはまだ開発途中であるため、一部のコマンドラインインターフェースが予告なく変更される可能性があります。ドキュメントや表示をよく読むようにしてください。

GitHubのリポジトリは[na4zagin3/satyrographos](https://github.com/na4zagin3/satyrographos)にあります。


OCamlによって記述され、OPAMによって管理されているライブラリを使用しているため、どちらもインストールする必要があります。
また、OPAMはWindowsとの相性が悪いため、WindowsでインストールするためにはWindows Subsystem for Linux(WSL)やCygwinなどを使う必要があります。
以下の説明では*nix環境があるという前提で説明をします。


## インストールするための環境を整える

上に説明している通り、Windows使用者はWSLやCygwinを事前にインストールしてください。

### OPAMのインストール

[OPAMのドキュメント](https://opam.ocaml.org/doc/Install.html)が存在しますが、ここでは必要な分だけ説明します。

#### Ubuntu18.04以降

ppaが用意されているのでこれを用います。

```sh
sudo add-apt-repository ppa:avsm/ppa
sudo apt update
sudo apt install opam
```

#### Ubuntu18.04以前

バイナリを直接インストールします。OCaml本体のインストールに必要なソフトウェアもついでにインストールします。

```sh
sudo apt-get update
sudo apt-get install build-essential git m4 unzip curl
sh <(curl -sL https://raw.githubusercontent.com/ocaml/opam/master/shell/install.sh)
```


#### macOS

Homebrewを使います。

```sh
brew install gpatch
brew install opam
```


### OCamlのインストール

インストールしたOPAMを使ってOCamlをインストールします。

一部のOSでは`sandbox`という機能が使えないらしいので、インストール時に`--disable-sandboxing`オプションが必要となります。詳しくは[FAQ](https://opam.ocaml.org/doc/FAQ.html#Why-does-opam-require-bwrap)を見てください。
この現象は少なくともWSL1では確認されていますので、WSL1を使用している人は必ずこのオプションを付けてください。WSL2では不要という情報もあります。それ以外の主要なOSでは必要ないはずです。
このオプションが必要なOSで`--disable-sandboxing`オプションを付けないでインストールしようとすると、FAQへのリンクの付いたエラーが出るのでわかりやすいと思います。


インストールするOCamlのバージョンは、執筆している2020年6月時点では最新版の4.10.0です。
SATySFiやsatyrographosのREADMEを読んで必要なバージョンを確かめてください。


#### WSLなど

```sh
opam init --comp 4.11.1 --disable-sandboxing
eval $(opam env)
```

#### それ以外


```sh
opam init --comp 4.11.1
eval $(opam env)
```

## 外部リポジトリを登録する

SATySFiもsatyrographosも外部リポジトリを使って独自のライブラリを利用します。
ですので、これを登録します。

```
opam repository add satysfi-external https://github.com/gfngfn/satysfi-external-repo.git

opam repository add satyrographos-repo https://github.com/na4zagin3/satyrographos-repo.git

opam update
```

## 本体をインストールする

satyrographos本体はOPAMに登録されているため、すぐにインストールすることができます。

```
opam depext satyrographos
opam install satyrographos
```


もし

```
No switch is currently set. Please use 'opam switch' to set or install a switch
```

のようなエラーが出た場合は

```
opam switch create 4.11.1
```

をするとエラーが解消されます。
