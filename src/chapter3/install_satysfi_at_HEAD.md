# SATySFiのHEAD版をインストールする

GitHubのリポジトリのHEAD版をインストールします。

## 環境構築

OPAMとOCamlがインストールされている必要があります。
satyrographosをインストールしていれば環境は自動で整っています。
もし、インストールしていないのであれば、「satyrographosをインストールする」を読んでOPAMとOCamlをインストールしてください。

## リポジトリをクローンする

GitHubにあるリポジトリを丸ごとクローンしてきます。

```sh
git clone https://github.com/gfngfn/SATySFi.git
```


## SATySFiをビルドする

クローンしてきたSATySFiフォルダに移動し、OPAMを使ってインストールします。
必要なライブラリは自動でインストールされます。

```sh
cd SATySFi

opam pin add satysfi .
opam install satysfi
```

## フォントや標準ライブラリをインストールする

フォントのインストールと標準ライブラリのインストールはそれぞれ別々のシェルスクリプトに分離されています。

`install-libs.sh`の実行には管理者権限が必要です。

```
./download-fonts.sh
sudo ./install-libs.sh
```

