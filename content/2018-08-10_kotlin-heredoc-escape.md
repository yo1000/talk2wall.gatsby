---
title: "Kotlinでヒアドキュメント中の文字をエスケープ"
date: "2018-08-10"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- kotlin
- spring
- spring-boot
- spel
redirect_from:
- /posts/2018-08-10-kotlin-heredoc-escape.html
---

Kotlin には変数展開機能があるため、`$`記号を使いたい場合エスケープが必要になる。
ヒアドキュメントと通常の文字列でエスケープの方法が少し異なり、クセがあるのでメモ。


## 要件
### 環境
今回の作業環境は以下のとおりです。

- Java 8
- Kotlin 1.2.41


## デモ
通常の文字列内でのエスケープ

```kotlin{numberLines:true}
println("\${xyz}") // OK: ${xyz}
```

ヒアドキュメント内でのエスケープ

```kotlin{numberLines:true}
println("""
\${xyz}
""".trimIndent()) // NG: Compile ERROR

println("""
\$\{xyz\}
""".trimIndent()) // NG: \$\{xyz\}

println("""
${'$'}{xyz}
""".trimIndent()) // OK: ${xyz}

val doller: Char = '$';
println("""
${doller}{xyz}
""".trimIndent()) // OK: ${xyz}
```

`Char`型の値として変数展開させた文字をくっつけて使う、というわけですね。どうしてヒアドキュメントだけエスケープ仕様違うんだろう🤔

SpringのSpEL等では、Kotlinでの変数展開同様に`$`記号がキーワードとして使われるため、この書き方は覚えておくとよさそうです。


## 参考
- http://kotlinlang.org/docs/reference/basic-types.html#string-templates
