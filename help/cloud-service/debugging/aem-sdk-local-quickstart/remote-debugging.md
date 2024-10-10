---
title: AEM SDK のリモートデバッグ
description: AEM SDK のローカルクイックスタートを使用すると、IDE からのリモート Java デバッグが可能になり、AEM でのライブコード実行を 1 ステップずつ進めて正確な実行フローを把握できるようになります。
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '270'
ht-degree: 100%

---

# AEM SDK のリモートデバッグ

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

AEM SDK のローカルクイックスタートを使用すると、IDE からのリモート Java デバッグが可能になり、AEM でのライブコード実行を 1 ステップずつ進めて正確な実行フローを把握できるようになります。

リモートデバッガーを AEM に接続するには、AEM SDK のローカルクイックスタートを特定のパラメーター（`-agentlib:...`）を使用して起動し、IDE から接続できるようにする必要があります。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ AEM SDK は Java 11 のみをサポートしています。
+ AEM がリモートデバッグ接続をリッスンするポートを `address` で指定します。これは、ローカル開発マシン上で使用可能な任意のポートに変更できます。
+ 最後のパラメーター（例：`aem-author-p4502.jar`）は AEM SKD Quickstart Jar です。AEM オーサーサービス（`aem-author-p4502.jar`）または AEM パブリッシュサービス（`aem-publish-p4503.jar`）のいずれかを指定できます。


## IDE セットアップ手順

ほとんどの Java IDE は Java プログラムのリモートデバッグをサポートしていますが、正確なセットアップ手順は各 IDE によって異なります。正確な手順については、ご利用の IDE のリモートデバッグセットアップ手順を確認してください。通常、IDE の設定には次の項目が必要です。

+ AEM SDK のローカルクイックスタートがリッスンしているホスト（`localhost`）。
+ AEM SDK のローカルクイックスタートがリモートデバッグ接続のためにリッスンしているポート（AEM SDK のローカルクイックスタートを起動する際に `address` パラメーターで指定されるポート）。
+ 時には、リモートデバッグの対象となるソースコードを提供する Maven プロジェクトを指定する必要があります。これは、OSGi バンドルの Maven プロジェクトです。

### セットアップ手順

+ [VS Code Java リモートデバッガーのセットアップ](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA リモートデバッガーのセットアップ](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse リモートデバッガーのセットアップ](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
