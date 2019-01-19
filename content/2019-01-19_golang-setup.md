---
title: "Goインストールメモ"
date: "2019-01-19"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- go
- beginner
---

RustでWebサーバーを書いていたのですが、このところ非同期IOまわりの実装が活発なようで、シンプルに書こうと思うと、まだ発展途上な部分が目立ったので、筆休めにGoをはじめてみました。

というわけで環境構築用の自分用メモです。


## GVMのインストール
GVMはNVMとかと同じ、ランタイムのバージョンマネージャ。

```bash
$ bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
Cloning from https://github.com/moovweb/gvm.git to /Users/xyz/.gvm
No existing Go versions detected
Installed GVM v1.0.22

Please restart your terminal session or to get started right away run
 `source /Users/xyz/.gvm/scripts/gvm`

$ source /Users/xyz/.gvm/scripts/gvm 
$ gvm version
Go Version Manager v1.0.22 installed at /Users/yo1000/.gvm
```


## Goのインストール
GVMを使って、Goをインストールする。

`gvm install`では、`-B`をつけないと、ソースからビルドしてインストールしようとするのですが、このビルド自体にGoが使われるため、初めてインストールする場合は`-B`オプションが必要になります。

```bash
$ gvm listall

gvm gos (available)

   ..
   go1.11.3
   go1.11.4
   go1.12beta1
   go1.12beta2
   ..


$ gvm install go1.11.4 -B
Installing go1.11.4 from binary source

$ gvm use go1.11.4
Now using version go1.11.4

$ go version
go version go1.11.4 darwin/amd64
```


## プロジェクトの作成とビルド
プロジェクトは任意のディレクトリを作成して、バイナリは任意のファイルを作成してビルドする。いろいろやってくれたりはしないが、特別なルールもない。

```bash
$ mkdir go-hello
$ cd go-hello/
$ echo '
package main
 
func main() {
    println("hello!")
}
'>main.go

$ go build
$ ./go-hello
hello!
```


## 参考
- https://qiita.com/makoto1007/items/9400d232f5673b34abda
