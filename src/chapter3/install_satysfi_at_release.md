# SATySFiのv0.0.6をインストールする

GitHubのリポジトリでリリースされているv0.0.6をインストールします。

## 環境構築

OPAMとOCamlがインストールされている必要があります。
satyrographosをインストールしていれば環境は自動で整っています。
もし、インストールしていないのであれば、「satyrographosをインストールする」を読んでOPAMとOCamlをインストールしてください。

## リリースされているファイルをダウンロードする

リリースされているファイルをダウンロードし、展開します。

```sh
curl https://github.com/gfngfn/SATySFi/archive/v0.0.6.zip
unzip v0.0.6.zip -d SATySFi
```

でSATySFiというフォルダが出来上がります。

## SATySFiをビルドする

作成したSATySFiフォルダに移動し、OPAMを使ってインストールします。
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

