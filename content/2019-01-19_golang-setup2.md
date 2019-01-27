---
title: "Goインストールメモやりなおし(vgo)"
date: "2019-01-27"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- go
- vgo
- beginner
---

いまのGoは、バイナリ内にパッケージマネージャが同梱されていることを知ったのでやりなおし。

`dep`なんていらなかった。`dep`だと、`GOPATH`が必要だったり、ディレクトリ構成もそこそこ縛られたりするし、`go`コマンドにバンドルされている`vgo`のほうが良かった。


## 環境要件
- Mac OS X 10.13.6

```
$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.13.6
BuildVersion:	17G65
```


## GVMのインストール
GVMはNVMとかと同じ、ランタイムのバージョンマネージャ。

```bash
$ bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
Cloning from https://github.com/moovweb/gvm.git to /Users/xyz/.gvm
No existing Go versions detected
Installed GVM v1.0.22

Please restart your terminal session or to get started right away run
 `source /Users/xyz/.gvm/scripts/gvm`

$ source ~/.gvm/scripts/gvm 
$ gvm version
Go Version Manager v1.0.22 installed at /Users/xyz/.gvm
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

$ echo "             
source ~/.gvm/scripts/gvm
gvm use `gvm list | grep -v gvm | grep -v '^ *$' | tr -d '=>' | tail`
">>~/.bash_profile 
```


## プロジェクトの作成とビルド
プロジェクトは任意のディレクトリを作成して、バイナリは任意のファイルを作成してビルドする。いろいろやってくれたりはしないが、特別なルールもない。
`go`コマンドに`vgo`がバンドルされているため、`go build`で依存解決も一緒にされる。

```bash
$ mkdir -p go-uuid
$ cd go-uuid

$ echo '
package main

import (
  "fmt"
  "github.com/satori/go.uuid"
)
 
func main() {
    fmt.Printf("%v\n", uuid.Must(uuid.NewV4()));
}
'>main.go

$ echo '
module xyz/go-uuid

require github.com/satori/go.uuid master
'>go.mod

$ go build
$ ./go-uuid
ca4d9324-5813-4ffe-a242-8a1ba6f7c200
```


## 参考
- https://qiita.com/makoto1007/items/9400d232f5673b34abda
- https://qiita.com/lufia/items/67701e2f927c77a75d6e
