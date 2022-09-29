---
title: Tomcat のインストールと設定
seo-title: Install and Configure Tomcat
description: これは、最初のインタラクティブ通信ドキュメントを作成するためのマルチステップチュートリアルの第 1 部です。この部分では、TOMCAT をインストールし、TOMCAT に sampleRest.war ファイルをデプロイします。
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# Tomcat のインストールと設定 {#install-and-configure-tomcat}

ここでは、TOMCAT をインストールし、TOMCAT に sampleRest.war ファイルをデプロイします。 この WAR ファイルで公開される REST エンドポイントは、データソースとフォームデータモデルの基礎となります。

Tomcat を設定するには、次の手順に従います。

1. JDK1.8 をダウンロードしてインストールします。
2. JAVA_HOME を JDK1.8 を指すように設定します。
3. ダウンロード [tomcat](https://tomcat.apache.org/). この war ファイルは、Tomcat バージョン 8.5.x および 9.0.x でテストされています。
4. 好みの tomcat バージョンをダウンロードします。 64 ビット版の windows zip は、コアセクションでダウンロードできます。
5. コンテンツをc:\tomcatに解凍します。
6. c ドライブには次のようなものが表示されます。 **c:\tomcat\apache-tomcat-8.5.27** tomcat のバージョンに応じて
7. 「CATALINA_HOME」という環境変数を作成し、その値を tomcat のインストールフォルダの例c:\tomcat\apache- tomcat-8.5.27 に設定します。
8. Tomcat インストールの webapps フォルダーに SampleRest.war ファイルをコピーします。
9. 新しいコマンドプロンプトウィンドウを起動します。
10. に移動します。 &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin をクリックし、startup.bat を実行します。
11. Tomcat が起動したら、次の方法で WAR ファイルによって公開されたエンドポイントをテストします。 [ここをクリック](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. この呼び出しの結果、サンプルデータが取得されるはずです。

おめでとう!!!。 tomcat を設定し、SampleRest.war ファイルをデプロイしました。
