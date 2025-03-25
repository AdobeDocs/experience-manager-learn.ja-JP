---
title: AEM forms での最初の OSGi バンドルの作成
description: Maven と Eclipse を使用して最初の OSGi バンドルを構築
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 177
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
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
*  [binary zip archive](http://maven.apache.org/download.cgi)をダウンロードします。
* zip アーカイブの内容を `c:\maven` に抽出します。
* `M2_HOME` という名前の環境変数を、`C:\maven\apache-maven-3.6.0` の値を使用して作成します。この **mvn** のバージョンは 3.6.0.です。この記事の作成時点での最新の Maven バージョンは 3.6.3 です。
* `%M2_HOME%\bin` をパスに追加します。
* 変更内容を保存します。
* 新しいコマンドプロンプトを開き、`mvn -version` と入力します。以下のスクリーンショットに示されるように、**mvn** バージョンが表示されます。

![data-source](assets/mvn-version.JPG)

## Settings.xml

Maven `settings.xml` ファイルは、様々な方法で Maven の実行を設定する値を定義します。通常は、ローカルリポジトリの場所、代替のリモートリポジトリサーバー、およびプライベートリポジトリの認証情報を定義するために使用されます。

`C:\Users\<username>\.m2 folder` に移動します。
[settings.zip](assets/settings.zip) ファイルのコンテンツを抽出し、`.m2` フォルダーに配置します。

## Eclipse のインストール

最新バージョンの [eclipse](https://www.eclipse.org/downloads/) をインストールします

## 最初のプロジェクトの作成

アーキタイプは、Maven プロジェクトテンプレートツールキットです。アーキタイプは、同じ種類の他のすべてのものの作成元となる、オリジナルのパターンまたはモデルとして定義されます。 アドビでは、一貫性のある Maven プロジェクト生成手段となるシステムを提供することを目指しているため、この名前は適切です。アーキタイプは、作成者がユーザー用の Maven プロジェクトテンプレートを作成する際に役立ち、ユーザーはこれを使用することで、これらのプロジェクトテンプレートのパラメーター化されたバージョンを生成できます。
最初の Maven プロジェクトを作成するには、次の手順に従います。

* C ドライブで `aemformsbundles` という名前の新しいフォルダーを作成します。
* コマンドプロンプトを開き、`c:\aemformsbundles` に移動します。
* コマンドプロンプトで次のコマンドを実行します。
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven プロジェクトがインタラクティブに生成され、次のような多数のプロパティに値を指定するよう求められます。

| プロパティ名 | 有意性 | 値 |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId は、すべてのプロジェクトで各プロジェクトを一意に識別します | com.learningaemforms.adobe |
| appsFolderName | プロジェクト構造を保持するフォルダーの名前 | learningaemforms |
| artifactId | artifactId は、バージョンのない jar の名前です。 作成した場合は、変な記号を含めることなく任意の名前を小文字で選択します。 | learningaemforms |
| version | 配布する場合は、数字とドット付きの一般的なバージョン（1.0、1.1、1.0.1 など）を選択できます。 | 1.0 |

Enter キーを押して、他のプロパティのデフォルト値をそのまま使用します。
すべてが正常に動作すると、コマンドウィンドウにビルド成功メッセージが表示されます

## Maven プロジェクトからの Eclipse プロジェクトの作成

作業ディレクトリを `learningaemforms` に変更します。
コマンドラインからの `mvn eclipse:eclipse` の実行
上記のコマンドは pom ファイルを読み取り、正しいメタデータで Eclipse プロジェクトを作成するため、Eclipse はプロジェクトのタイプ、関係、クラスパスなどを認識できるようになります。

## Eclipse への プロジェクトの読み込み

**Eclipse** を起動します

ここに示すように、**ファイル／読み込み**&#x200B;に移動し、「**既存 Maven プロジェクト**」を選択します

![data-source](assets/import-mvn-project.JPG)

「次へ」をクリックします。

「**参照**」ボタンをクリックして「`c:\aemformsbundles\learningaemform`」を選択します。

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>必要に応じて、適切なモジュールを読み込むように選択できます。 プロジェクトで Java コードを作成するだけの場合は、コアモジュールのみを選択して読み込みます。

読み込み処理を開始するには「**終了**」をクリックします

プロジェクトが Eclipse に読み込まれると、複数の `learningaemforms.xxxx` フォルダーができます

`learningaemforms.core` フォルダーの下にある `src/main/java` を展開します。コードの大部分をこのフォルダー内に記述することになります。

![data-source](assets/learning-core.JPG)

## プロジェクトを構築する

OSGi サービスまたはサーブレットを記述したら、Felix web コンソールを使用してデプロイできる OSGi バンドルを生成するために、プロジェクトを構築する必要があります。 Maven プロジェクトに適切なクライアント SDK を含めるには、[AEMFD クライアント SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/)を参照してください。以下のように、コアプロジェクトの `pom.xml` の依存関係セクションに AEM FD Client SDK を含める必要があります。 

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

プロジェクトを構築するには、次の手順に従います。

* **コマンドプロンプトウィンドウ**&#x200B;を開きます
* `c:\aemformsbundles\learningaemforms\core` に移動します。
* コマンド `mvn clean install` を実行します
すべて正常に完了すると、次の場所の `C:\AEMFormsBundles\learningaemforms\core\target` にバンドルが表示されます。これで、Felix web コンソールを使用してこのバンドルを AEM にデプロイする準備が整いました。
