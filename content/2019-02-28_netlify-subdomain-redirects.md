---
title: "Netlifyのサブドメインをプライマリドメインにリダイレクトする"
date: "2019-02-28"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- netlify
---

Netlifyでは、自分のドメインを持っていなくとも、`netlify.com`のサブドメインとして、デプロイしたサイトが公開されます。ところが、自分のドメインをすでに持っており、カスタムドメインを設定しているような場合、同一コンテンツが複数のドメインで同時に配信されることになってしまいます。

このような状態に対し、SEO上の懸念を感じた場合にしておくとよいのが、リダイレクト設定です。


## 環境要件
- Netlify 2019-02-28現在


## リダイレクトファイルを配置する
Netlifyでは、公開ディレクトリのルートに`_redirects`ファイルを配置することで、リダイレクトルールを定義することができます。

例えばGatsbyJSを使ったブログでは、`static`ディレクトリに配置したファイルは、ビルドするとそのまま`public`ディレクトリに出力され、これが公開ディレクトリとなります。そのため、`static`ディレクトリに、`_redirects`ファイルを配置することで、リダイレクトルールを定義、適用できるようになります。

```
$ find . -name _redirects
./public/_redirects
./public/gatsby-material-starter/_redirects
./static/_redirects
```


## リダイレクトルールを定義する
NetlifyサブドメインのURLに向けられたリクエストを、自分のドメインにリダイレクトさせるようにするには、以下のようなルールを記述します。

```
https://xyzxyz.netlify.com/* https://your.domain.com/:splat 301!
```

見たままですが、この例のようなルールを適用すると、`https://xyzxyz.netlify.com/`にリクエストがあった場合に、`https://your.domain.com/`にリダイレクトされるようになります。リダイレクト先のURLに`:slpat`を記述しておくと、リダイレクト元の`*`で受け取ったパスやクエリ文字列を、リダイレクト先に渡してくれるようになります。

なお、自分のサイトに適用可能なリダイレクトルールのサンプルについては、Netlifyのサイト上から確認できるので、これをそのまま使うこともできます。確認は以下から。

https://app.netlify.com/ > _(対象のドメイン)_ > `Domain settings` > `How to redirect the default Netlify subdomain to your primary domain`


## 参考
- https://www.netlify.com/docs/redirects/
- https://github.com/gatsbyjs/gatsby/issues/6268
