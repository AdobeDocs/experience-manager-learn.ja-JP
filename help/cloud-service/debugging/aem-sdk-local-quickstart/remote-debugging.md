---
title: AEM SDKのリモートデバッグ
description: AEM SDKのローカルクイックスタートを使用すると、IDEからのリモートJavaデバッグが可能になり、AEMでのライブコードの実行手順を進めて、正確な実行フローを理解できます。
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# AEM SDKのリモートデバッグ

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDKのローカルクイックスタートを使用すると、IDEからのリモートJavaデバッグが可能になり、AEMでのライブコードの実行手順を進めて、正確な実行フローを理解できます。

リモートデバッガーをAEMに接続するには、AEM SDKのローカルクイックスタートを、IDEが接続できる特定のパラメーター(`-agentlib:...`)で起動する必要があります。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` は、リモートデバッグ接続をリッスンするポートAEMを指定します。ポートは、ローカル開発マシン上の使用可能な任意のポートに変更できます。
+ 最後のパラメータ(例： `aem-author-p4502.jar`)はAEM SKD Quickstart Jarです。AEMオーサーサービス(`aem-author-p4502.jar`)またはAEMパブリッシュサービス(`aem-publish-p4503.jar`)のどちらかです。

## IDEの設定手順

ほとんどのJava IDEは、Javaプログラムのリモートデバッグをサポートしていますが、各IDEの正確な設定手順は異なります。 正確な手順については、IDEのリモートデバッグ設定の手順を確認してください。 通常、IDEの構成には次が必要です。

+ ホストAEM SDKのローカルクイックスタートがリッスンしています(`localhost`)。
+ AEM SDKのローカルクイックスタートが、リモートデバッグ接続をリッスンしています。リモートデバッグ接続は、AEM SDKのローカルクイックスタートを開始する際に`address`パラメーターで指定されるポートです。
+ リモートデバッグにソースコードを提供するMavenプロジェクトを指定する必要が生じる場合があります。これは、OSGiバンドルmavenプロジェクトプロジェクトです。

### 手順の設定

+ [VS Code Javaリモートデバッガの設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEAリモートデバッガの設定](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Eclipseリモートデバッガーの設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
