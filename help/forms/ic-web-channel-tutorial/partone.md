---
title: Tomcat のインストールと設定
description: これは、初めてのインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第 1 部です。このパートでは、TOMCAT をインストールし、TOMCAT に sampleRest.war ファイルをデプロイします。
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 44
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 100%

---

# Tomcat のインストールと設定 {#install-and-configure-tomcat}

このパートでは、TOMCAT をインストールし、TOMCAT に sampleRest.war ファイルをデプロイします。この WAR ファイルで公開される REST エンドポイントは、このチュートリアルにおけるデータソースとフォームデータモデルの基礎となります。

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

## 次の手順

[RESTful データソースの設定](./parttwo.md)
