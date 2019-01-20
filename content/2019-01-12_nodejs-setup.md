---
title: "NodeJSインストールメモ"
date: "2019-01-12"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- nodejs
- nvm
- gatsby
- beginner
---

環境移ったときなど、毎回調べ直している気がしたので自分用メモ。

## NVMのインストール

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
source ~/.bash_profile

nvm --version
0.34.0
```

## NodeJSのインストール

```bash
nvm install 11.0.0

node -v
v11.0.0

npm -v
6.4.1
```

## NPMの更新

```bash
npm -g update

npm -v
6.5.0
```

## Gatsby Clientのインストール
これは使わなければ、とくに用事なし。

```bash
npm -g install gatsby-cli

gatsby -v
2.4.8
```

## プロジェクトのビルド
`XYZ`の部分は、プロジェクトに応じて変更する。

```bash
npm install
npm run XYZ
```

