---
title: "Rustインストールメモ"
date: "2019-01-13"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- rust
- cargo
- beginner
---

環境移ったときなど、毎回調べ直している気がしたので自分用メモ。読み方は`rˈʌst`。


## Rustupのインストール
RustupはNVMとかと同じ、ランタイムのバージョンマネージャ。

```bash
curl https://sh.rustup.rs -sSf | sh
..
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>1

source ~/.cargo/env

rustup -V
rustup 1.16.0 (beab5ac2b 2018-12-06)
```


## Rustのインストール
Rustupを使って、Rustをインストールする。

```bash
rustup install stable

rustc -V
rustc 1.31.1 (b6c32da9b 2018-12-18)

cargo -V
cargo 1.31.0 (339d9f9c8 2018-11-16)
```


## プロジェクトの作成とビルド
プロジェクト作成時に、Hello Worldまでは自動で作成してくれる。

```bash
cargo new --bin rust-hello
     Created binary (application) `rust-hello` package

cd rust-hello/

cargo run --release
   Compiling rust-hello v0.1.0 (/xyz/rust-hello)
    Finished release [optimized] target(s) in 1.39s
     Running `target/release/rust-hello`
Hello, world!
```


## 参考
- https://www.rust-lang.org/tools/install
- https://dev.classmethod.jp/server-side/language/rust-setup-with-rustup/
