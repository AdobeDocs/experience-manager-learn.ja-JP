---
title: Tomcat のインストールと設定ビデオ
description: これは、最初のインタラクティブなコミュニケーションドキュメントを作成するためのマルチステップチュートリアルの第 1 部です。
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
duration: 237
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 100%

---

# Tomcat のインストールと設定 {#install-and-configure-tomcat}

このパートでは、TOMCAT をインストールし、TOMCAT に sampleRest.war ファイルをデプロイします。この WAR ファイルで公開される REST エンドポイントは、このチュートリアルにおけるデータソースとフォームデータモデルの基礎となります。

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

Tomcat をセットアップするには、次の手順に従います。

1. JDK1.8 をダウンロードしてインストールします。
2. JDK1.8 を指すように JAVA_HOME を設定します。
3. [tomcat](https://tomcat.apache.org/) をダウンロードします。この war ファイルは、Tomcat バージョン 8.5.x および 9.0.x でテストされています。
4. 好みの Tomcat バージョンをダウンロードします。コアセクションの 64 ビット Windows 版の zip をダウンロードします。
5. 内容を C:\tomcat に解凍します。
6. Tomcat のバージョンに応じて、C ドライブの **C:\tomcat\apache-tomcat-8.5.27** に次のようなものが表示されます。
7. 「CATALINA_HOME」という環境変数を作成し、その値を Tomcat のインストールフォルダーの例（C:\tomcat\apache- tomcat-8.5.27）に設定します。
8. Tomcat インストールの webapps フォルダーに SampleRest.war ファイルをコピーします。
9. 新しいコマンドプロンプトウィンドウを起動します。
10. &lt;tomcat install folder>\bin に移動し、startup.bat を実行します。
11. Tomcat が起動したら、[こちらをクリック](http://localhost:8080/SampleRest/webapi/getStatement/9586)して、WAR ファイルによって公開されたエンドポイントをテストします。
12. この呼び出しの結果、サンプルデータが取得されます。

おめでとうございます。Tomcat をセットアップし、SampleRest.war ファイルをデプロイしました。

次のビデオでは、Tomcat でのサンプルアプリケーションのデプロイメントについて説明します
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

## 次の手順

[RESTful データソースの作成](./create-data-source.md)