---
title: "SlackとJIRAを連携する"
date: "2019-02-15"
cover: "cover-tech.jpg"
category: "Tech"
tags:
- slack
- jira
- jira-cloud
---

SlackとJIRAを連携するには、Jira Cloudを使うか、Jira Server Alertsを使うことになります。どちらでも連携自体は可能ですが、通知内容はJira Cloudのほうがよりリッチで、Slackを離れることなくIssueを把握しやすいです。

ところが、Jira Cloudは、セットアップ方法の説明がSlack側のドキュメントだけで完結しておらず、少々分かりづらかったりします。まあドキュメントの在り処さえわかってしまえば、難しい話ではないのですが。ドキュメントはそれぞれ以下にあります。

- [Jira と Slack を連携させる - Jira Cloud](https://get.slack.help/hc/ja/articles/218475657-Jira-%E3%81%A8-Slack-%E3%82%92%E9%80%A3%E6%90%BA%E3%81%95%E3%81%9B%E3%82%8B#jira-cloud-1)
- ["Connecting with this Jira Cloud instance was disabled by administrator" というメッセージが表示されます。なぜでしょうか。](https://ja.confluence.atlassian.com/jirasoftwarecloud/jira-cloud-for-slack-950819054.html#JiraCloudforSlack-WhyamIseeingthemessage%22ConnectingwiththisJiraCloudinstancewasdisabledbyadministrator%22?)

ドキュメントの要約になりますが、以下に手順をまとめておきます。


## 環境要件
- Slack 2019-02-15現在
- Atlassian JIRA 2019-02-15現在
- 連携するJIRAのサイト管理者権限


## Slack Jira Cloudアプリのインストール
1. Slack の App ディレクトリで Jira Cloud ページにアクセスします。
2. ページ上部の「Slack に追加」ボタンをクリックします。
3. Jira Cloud で許可したい Slack ワークスペースを選択します。
4. Authorize (許可する) をクリックします。


## JIRA Slack integrationの設定
`https://<instancename>.atlassian.net/plugins/servlet/ac/jira-slack-integration/addon-config-page`にアクセスして、`Enable Slack integration`を有効にします。


## SlackでJIRAに接続
Slack上の連携したいチャネル上で、`/jira connect <自分の Jira URL>`コマンドを発言します。`<自分の Jira URL>`には、`****.atlassian.net`のようなURLが入ります。

接続が成功するとJIRA Botから、以下のようなメッセージが届くので、`Verify your Jira account`のリンクを踏んで、連携設定を完了させます。


## 参考
- [Jira と Slack を連携させる - Jira Cloud](https://get.slack.help/hc/ja/articles/218475657-Jira-%E3%81%A8-Slack-%E3%82%92%E9%80%A3%E6%90%BA%E3%81%95%E3%81%9B%E3%82%8B#jira-cloud-1)
- ["Connecting with this Jira Cloud instance was disabled by administrator" というメッセージが表示されます。なぜでしょうか。](https://ja.confluence.atlassian.com/jirasoftwarecloud/jira-cloud-for-slack-950819054.html#JiraCloudforSlack-WhyamIseeingthemessage%22ConnectingwiththisJiraCloudinstancewasdisabledbyadministrator%22?)
