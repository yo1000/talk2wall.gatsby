---
title: "KotlinでJPA使用時にデフォルトコンストラクタ要求を回避する"
date: "2019-02-27"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- kotlin
- jpa
- spring
- spring-boot
---

KotlinでJPAを使用する場合、エンティティクラスにデフォルトコンストラクタが存在しないと、以下のような例外がスローされます。

> org.springframework.orm.jpa.JpaSystemException: No default constructor for entity:  : .. ; nested exception is org.hibernate.InstantiationException: No default constructor for entity:  : ..

これはJPA実装のひとつであるHibernateが、エンティティクラスのインスタンス生成時に、引数付きコンストラクタではなく、デフォルトコンストラクタを使おうとするために発生する例外ですが、Kotlinでは、コンストラクタとプロパティを一緒に記述できるため、多くの場合デフォルトコンストラクタが用意されません。

Kotlinでデフォルトコンストラクタを作成したい場合、すべてのコンストラクタ引数に初期値を与えるか、コンストラクタ引数を意図的に排除するかしなければなりません。しかし、こうなってしまうと、Kotlinの良さであるイミュータブルに記述しやすい特徴や、NULLを回避しやすい特徴との相性が悪くなり、これらを十分に活かせなくなってしまいます。そこでKotlinらしさを損なわず、JPAを使うにはどのようにすればよいか、というのが今回の内容です。


## 環境要件
- Oracle Java 1.8.0_161
- Apache Maven 3.6.0
- Kotlin 1.2.71
- Spring Boot 2.1.3.RELEASE

```
$ ./mvnw -version
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-25T03:41:47+09:00)
Maven home: ~/.m2/wrapper/dists/apache-maven-3.6.0-bin/2dakv70gp803gtm5ve1ufmvttn/apache-maven-3.6.0
Java version: 1.8.0_161, vendor: Oracle Corporation, runtime: /Library/Java/JavaVirtualMachines/jdk1.8.0_161.jdk/Contents/Home/jre
Default locale: ja_JP, platform encoding: UTF-8
OS name: "mac os x", version: "10.13.6", arch: "x86_64", family: "mac"
```


## kotlin-maven-noargを適用する
実は対応自体はかんたんで、実装を変更する必要はまったくなく、Mavenのビルド構成に手を加えるだけで済んでしまいます。`pom.xml`を編集して、以下のようにします。

```xml{numberLines:true}{12,21-25}
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <configuration>
                <args>
                    <arg>-Xjsr305=strict</arg>
                </args>
                <compilerPlugins>
                    <plugin>spring</plugin>
                    <plugin>jpa</plugin>
                </compilerPlugins>
            </configuration>
            <dependencies>
                <dependency>
                    <groupId>org.jetbrains.kotlin</groupId>
                    <artifactId>kotlin-maven-allopen</artifactId>
                    <version>${kotlin.version}</version>
                </dependency>
                <dependency>
                    <groupId>org.jetbrains.kotlin</groupId>
                    <artifactId>kotlin-maven-noarg</artifactId>
                    <version>${kotlin.version}</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```

`kotlin-maven-noarg`を依存に追加して、`compilerPlugins`に`jpa`を追加します。

これだけで、`@Entity`, `@Embeddable`および`@MappedSuperclass`アノテーションが付与されたクラスに対して、コンパイル時にデフォルトコンストラクタが自動生成されるようになり、Kotlinの良さを活かしたまま、JPAを使えるようになります。


## 参考
- https://kotlinlang.org/docs/reference/compiler-plugins.html#jpa-support
- https://stackoverflow.com/questions/32038177/kotlin-with-jpa-default-constructor-hell