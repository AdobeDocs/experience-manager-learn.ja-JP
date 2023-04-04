---
title: AEM SDK のリモートデバッグ
description: AEM SDK のローカルクイックスタートを使用すると、IDE からのリモート Java デバッグが可能になり、AEMでのライブコードの実行手順を進めて、正確な実行フローを理解できます。
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---

# AEM SDK のリモートデバッグ

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

AEM SDK のローカルクイックスタートを使用すると、IDE からのリモート Java デバッグが可能になり、AEMでのライブコードの実行手順を進めて、正確な実行フローを理解できます。

リモートデバッガーをAEMに接続するには、AEM SDK のローカルクイックスタートを特定のパラメーター (`-agentlib:...`) を使用して、IDE が接続できるようになります。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` ポートAEMがリモートデバッグ接続をリッスンするように指定します。このポートは、ローカル開発マシン上の使用可能なポートに変更できます。
+ 最後のパラメータ ( 例： `aem-author-p4502.jar`) はAEM SKD Quickstart Jar です。 AEM オーサーサービス (`aem-author-p4502.jar`) または AEM パブリッシュサービス (`aem-publish-p4503.jar`) をクリックします。

## IDE の設定手順

ほとんどの Java IDE は Java プログラムのリモートデバッグをサポートしていますが、各 IDE の正確な設定手順は異なります。 正確な手順については、IDE のリモートデバッグ設定の手順を確認してください。 通常、IDE の構成には次の項目が必要です。

+ ホストAEM SDK のローカルクイックスタートがリッスンしています ( つまり、 `localhost`.
+ ポートAEM SDK のローカルクイックスタートが、リモートデバッグ接続 ( `address` AEM SDK のローカルクイックスタートを開始する際のパラメーター。
+ リモートデバッグにソースコードを提供する Maven プロジェクトを指定する必要が生じる場合があります。これは、OSGi バンドル maven プロジェクトプロジェクトです。

### 手順の設定

+ [VS Code Java リモートデバッガの設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEA リモートデバッガの設定](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipse リモートデバッガーの設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
