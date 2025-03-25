---
title: AEM Forms での最初の OSGi バンドルの作成
description: Maven と Eclipse を使用して最初の OSGi バンドルを構築
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
duration: 145
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 100%

---

# 初めての OSGi バンドルの作成

OSGi バンドルは、Java コード、リソース、およびバンドルとその依存関係を記述するマニフェストを含む Java™アーカイブファイルです。 バンドルは、アプリケーションのデプロイメント単位です。 この記事は、AEM Forms 6.4 または 6.5 を使用して OSGi サービスまたはサーブレットを作成する開発者向けです。最初の OSGi バンドルを構築するには、次の手順に従います。


## JDK のインストール

サポートされているバージョンの JDK をインストールします。 ここでは、JDK1.8 を使用しています。環境変数に **JAVA_HOME** を追加し、JDK インストールのルートフォルダーをポイントしている必要があります。
%JAVA_HOME%/bin をパスに追加します。

![data-source](assets/java-home.JPG)

>[!NOTE]
> JDK 15 は使用しないでください。 AEMでサポートされていません。

### JDK バージョンのテスト

新しいコマンドプロンプトウィンドウを開き、`java -version` と入力します。`JAVA_HOME` 変数で識別されている JDK のバージョンに戻る必要があります。

![data-source](assets/java-version.JPG)

## Maven のインストール

Maven は、主に Java プロジェクトで使用されるビルド自動処理ツールです。次の手順に従って、ローカルシステムに Maven をインストールします。

* C ドライブに `maven` という名前のフォルダーを作成します。
*  [binary zip archive](https://maven.apache.org/download.cgi)をダウンロードします。
* zip アーカイブの内容を `c:\maven` に抽出します。
* `M2_HOME` という名前の環境変数を、`C:\maven\apache-maven-3.6.0` の値を使用して作成します。この **mvn** のバージョンは 3.6.0.です。この記事の作成時点での最新の Maven バージョンは 3.6.3 です。
* `%M2_HOME%\bin` をパスに追加します。
* 変更内容を保存します。
* 新しいコマンドプロンプトを開き、`mvn -version` と入力します。以下のスクリーンショットに示されるように、**mvn** バージョンが表示されます。

![data-source](assets/mvn-version.JPG)


## Eclipse のインストール

最新バージョンの [eclipse](https://www.eclipse.org/downloads/) をインストールします

## 最初のプロジェクトの作成

アーキタイプは、Maven プロジェクトテンプレートツールキットです。アーキタイプは、同じ種類の他のすべてのものの作成元となる、オリジナルのパターンまたはモデルとして定義されます。 アドビでは、一貫性のある Maven プロジェクト生成手段となるシステムを提供することを目指しているため、この名前は適切です。アーキタイプは、作成者がユーザー用の Maven プロジェクトテンプレートを作成する際に役立ち、ユーザーはこれを使用することで、これらのプロジェクトテンプレートのパラメーター化されたバージョンを生成できます。
最初の Maven プロジェクトを作成するには、次の手順に従います。

* C ドライブで `aemformsbundles` という名前の新しいフォルダーを作成します。
* コマンドプロンプトを開き、`c:\aemformsbundles` に移動します。
* コマンドプロンプトで次のコマンドを実行します。

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

正常に完了すると、コマンドウィンドウにビルド成功メッセージが表示されます

## Maven プロジェクトからの Eclipse プロジェクトの作成

* 作業ディレクトリを `mysite` に変更します
* コマンドラインから `mvn eclipse:eclipse` を実行します。このコマンドは、POM ファイルを読み取り、正しいメタデータを使用して Eclipse プロジェクトを作成し、Eclipse がプロジェクトのタイプ、関係、クラスパスなどを認識できるようにします。

## Eclipse への プロジェクトの読み込み

**Eclipse** を起動します

ここに示すように、**ファイル／読み込み**&#x200B;に移動し、「**既存 Maven プロジェクト**」を選択します

![data-source](assets/import-mvn-project.JPG)

「次へ」をクリックします。

「**参照**」ボタンをクリックして、c:\aemformsbundles\mysite を選択します

![data-source](assets/mysite-eclipse-project.png)

>[!NOTE]
>必要に応じて、適切なモジュールを読み込むように選択できます。 プロジェクトで Java コードを作成するだけの場合は、コアモジュールのみを選択して読み込みます。

読み込み処理を開始するには「**終了**」をクリックします

プロジェクトが Eclipse に読み込まれると、複数の `mysite.xxxx` フォルダーができます

`mysite.core` フォルダーの下にある `src/main/java` を展開します。コードの大部分をこのフォルダー内に記述することになります。

![data-source](assets/mysite-core-project.png)

## AEMFD Client SDK を含めます

AEM Forms に付属する様々なサービスを活用するには、AEMFD Client SDK をプロジェクトに含める必要があります。 [AEMFD Client SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) を参照して、Maven プロジェクトに適切なクライアント SDK を含めてください。以下に示すように、コアプロジェクトの `pom.xml` の依存関係セクションに AEM FD Client SDK を含める必要があります。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

プロジェクトを構築するには、次の手順に従います。

* **コマンドプロンプトウィンドウ**&#x200B;を開きます
* `c:\aemformsbundles\mysite\core` に移動します。
* `mvn clean install -PautoInstallBundle` コマンドを実行します。 
上記のコマンドは、バンドルをビルドして `http://localhost:4502` で実行されている AEM サーバーにインストールします。バンドルは、ファイルシステムの
  `C:\AEMFormsBundles\mysite\core\target` からも利用可能で、[Felix web コンソール](http://localhost:4502/system/console/bundles)を使用してデプロイできます

## 次の手順

[OSGi サービスの作成](./create-osgi-service.md)

