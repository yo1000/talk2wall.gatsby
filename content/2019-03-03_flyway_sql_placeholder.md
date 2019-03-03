---
title: "FlywayのSQLにはプレースホルダが使える"
date: "2019-03-03"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- spring-boot
- flyway
- sql
---

FlywayによるDBマイグレーションで、ユーザーテーブルのようなものをセットアップしたい場合に、SQLに直接パスワードを書きたくないなと調べたところ、プレースホルダーを使えることがわかったので、そのメモです。Spring Bootを組み合わせると、さらに便利に使えるようになるので、今回はSpring Bootを使ったサンプルになります。


## 環境要件
- Oracle JDK 1.8.0_161
- Kotlin 1.2.71
- Spring Boot 2.1.3.RELEASE
- Flyway 5.2.4


## プレースホルダーの書き方
プレースホルダーを使うには、変数化したい箇所を`${variable.name}`のように書き換えるだけです。はじめに書いたような、パスワードをSQLから追い出したいような場合は、以下のように書きます。なお今回のサンプルは、MySQLで確認しています。

```sql{numberLines:true}{12}
INSERT INTO `users`(
    `id`,
    `username`,
    `password`,
    `role`,
    `expires_at`,
    `creator_user_id`,
    `created_at`
) VALUES (
    'XXXXXXXX-XXXX-4XXX-rXXX-XXXXXXXXXXXX',
    'admin',
    SHA2('${init.password}', 256),
    'admin',
    null,
    'XXXXXXXX-XXXX-4XXX-rXXX-XXXXXXXXXXXX',
    NOW()
);
```


## プレースホルダーへ値を渡す
プレースホルダーへ値を渡すには、`flyway.conf`に変数名と値のマッピングを設定します。このほか、Spring Bootを使っている場合は、`application.properties`および、`application.yml`に設定することでも値を渡せます。

今回はSpring Bootで、プレースホルダーへ値を渡す例を見てみます。設定は非常に簡単で、以下のように、`application.yml`の`flyway.placeholders.*`にプレースホルダー名と一致するエントリを追加して、値を設定するだけで良いです。

```yaml{numberLines:true}{8-10}
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/testdb
    username: testuser
    password: testpassword
  flyway:
    placeholders:
      # SHA2('${init.password}', 256)
      init.password: "secret!123"
```

Spring Bootに慣れている方ならここでもうお気づきかと思われますが、`application.yml`に値を設定できるということはつまり、環境変数や、システムプロパティ、実行時引数、他、様々なパラメーターソースから値を受け渡せるということで、非常に柔軟にプレースホルダーを活用することができます。


## 参考
- https://flywaydb.org/documentation/migrations#placeholder-replacement
- https://flywaydb.org/documentation/configfiles
- https://stackoverflow.com/a/49161920/5610904
