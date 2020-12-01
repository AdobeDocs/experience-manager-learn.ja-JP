---
title: AEM SDKのリモートデバッグ
description: AEM SDKのローカルクイックスタートでは、IDEからのリモートJavaデバッグが可能で、AEMでのライブコード実行を順を追って実行フローを正確に把握できます。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# AEM SDKのリモートデバッグ

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

AEM SDKのローカルクイックスタートでは、IDEからのリモートJavaデバッグが可能で、AEMでのライブコード実行を順を追って実行フローを正確に把握できます。

リモートデバッガをAEMに接続するには、AEM SDKのローカルクイックスタートを特定のパラメータ(`-agentlib:...`)で開始し、IDEがそれに接続できるようにする必要があります。

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` は、ポートAEMがリモートデバッグ接続をリッスンするように指定します。ローカル開発マシン上の使用可能なポートに変更できます。
+ 最後のパラメーター( `aem-author-p4502.jar`)はAEM SDK Quickstart Jarです。これは、AEM Authorサービス(`aem-author-p4502.jar`)またはAEM Publishサービス(`aem-publish-p4503.jar`)のいずれかです。

## IDEの設定手順

ほとんどのJava IDEはJavaプログラムのリモートデバッグをサポートしていますが、各IDEの正確な設定手順は異なります。 IDEのリモートデバッグの設定手順を正確な手順で確認してください。 通常、IDEの構成には次の要件があります。

+ ホストAEM SDKのローカルクイックスタートがリッスンしています(`localhost`)。
+ ポートAEM SDKのローカルクイックスタートは、リモートデバッグ接続をリッスンしています。これは、AEM SDKのローカルクイックスタートを開始する際に`address`パラメーターで指定されたポートです。
+ 場合によっては、リモートデバッグにソースコードを提供するMavenプロジェクトを指定する必要があります。これは、OSGiバンドルmavenプロジェクトです。

### 手順の設定

+ [VSコードJavaリモートデバッガの設定](https://code.visualstudio.com/docs/java/java-debugging)
+ [IntelliJ IDEAリモートデバッガの設定](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Eclipseリモートデバッガの設定](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
