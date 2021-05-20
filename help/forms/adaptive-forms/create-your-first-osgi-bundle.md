---
title: AEM formsでの最初のOSGiバンドルの作成
description: MavenとEclipseを使用して最初のOSGiバンドルを構築
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 3%

---


# 最初のOSGiバンドルを作成します。

OSGiバンドルは、Javaコード、リソース、バンドルとその依存関係を記述するマニフェストを含むJava™アーカイブファイルです。 バンドルは、アプリケーションのデプロイメント単位です。 この記事は、AEM Forms 6.4または6.5を使用してOSGiサービスまたはサーブレットを作成する開発者向けです。最初のOSGiバンドルを構築するには、次の手順に従います。


## JDK のインストール

サポートされているバージョンのJDKをインストールします。 JDK1.8を使用しています。環境変数に&#x200B;**JAVA_HOME**が追加され、JDKインストールのルートフォルダーを指していることを確認してください。
%JAVA_HOME%/binをパスに追加します

![data-source](assets/java-home.JPG)

>[!NOTE]
> JDK 15は使用しないでください。 AEMではサポートされていません。

### JDKバージョンのテスト

新しいコマンドプロンプトウィンドウを開き、次のように入力します。`java -version`. `JAVA_HOME`変数で識別されるJDKのバージョンを戻す必要があります

![data-source](assets/java-version.JPG)

## Mavenのインストール

Mavenは、主にJavaプロジェクトで使用されるビルド自動化ツールです。 次の手順に従って、ローカルシステムにmavenをインストールします。

* Cドライブに`maven`という名前のフォルダーを作成します。
* [バイナリzipアーカイブ](http://maven.apache.org/download.cgi)をダウンロードします。
* zipアーカイブの内容を`c:\maven`に展開します。
* `M2_HOME`という環境変数を作成し、値を`C:\maven\apache-maven-3.6.0`にします。 私の場合、**mvn**&#x200B;のバージョンは3.6.0です。この記事を書く時点で、最新のMavenのバージョンは3.6.3です
* パスに`%M2_HOME%\bin`を追加します。
* 変更を保存します
* 新しいコマンドプロンプトを開き、`mvn -version`と入力します。 **mvn**&#x200B;バージョンが次のスクリーンショットに示すように表示されます

![data-source](assets/mvn-version.JPG)

## Settings.xml

Mavenの`settings.xml`ファイルは、Mavenの実行を様々な方法で設定する値を定義します。 最も一般的に、ローカルリポジトリの場所、代替リモートリポジトリサーバー、およびプライベートリポジトリの認証情報を定義するために使用されます。

`C:\Users\<username>\.m2 folder`に移動します。
[settings.zip](assets/settings.zip)ファイルの内容を抽出し、`.m2`フォルダーに配置します。

## Eclipse のインストール

最新バージョンの[eclipse](https://www.eclipse.org/downloads/)をインストールします。

## 最初のプロジェクトを作成する

アーキタイプは、Mavenプロジェクトテンプレートツールキットです。 アーキタイプは、同じ種類の他のすべてのものを作成する元のパターンまたはモデルとして定義されます。 Mavenプロジェクトを生成する一貫した手段を提供するシステムを提供しようとするときに、名前が適しています。 アーキタイプは、作成者がユーザー用のMavenプロジェクトテンプレートを作成するのに役立ち、これらのプロジェクトテンプレートのパラメーター化バージョンを生成する手段をユーザーに提供します。
最初のMavenプロジェクトを作成するには、次の手順に従います。

* Cドライブに`aemformsbundles`という新しいフォルダーを作成します。
* コマンドプロンプトを開き、`c:\aemformsbundles`に移動します。
* コマンドプロンプトで次のコマンドを実行します。
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Mavenプロジェクトはインタラクティブに生成され、次のような様々なプロパティに値を指定するよう求められます。

| プロパティ名 | 有意性 | 値 |
------------------------|---------------------------------------|---------------------
| groupId | groupIdは、すべてのプロジェクトでプロジェクトを一意に識別します | com.learningaemforms.adobe |
| appsFolderName | プロジェクト構造を保持するフォルダーの名前 | 学習フォーム |
| artifactId | artifactIdは、バージョンのないjarの名前です。 作成した場合は、小文字で変な記号を使用せずに、任意の名前を選択できます。 | 学習フォーム |
| version | 配布する場合は、数値とドット（1.0、1.1、1.0.1など）を含む一般的なバージョンを選択できます。 | 1.0 |

Enterキーを押して、その他のプロパティのデフォルト値を受け入れます。
すべて正常に動作する場合は、コマンドウィンドウにビルド成功メッセージが表示されます

## MavenプロジェクトからEclipseプロジェクトを作成する

作業ディレクトリを`learningaemforms`に変更します。
コマンドラインから`mvn eclipse:eclipse`を実行する
上記のコマンドは、POMファイルを読み取り、正しいメタデータを使用してEclipseプロジェクトを作成し、Eclipseがプロジェクトの種類、関係、クラスパスなどを認識できるようにします。

## プロジェクトをEclipseに読み込む

**Eclipse**&#x200B;を起動

**ファイル/読み込み**&#x200B;に移動し、次に示すように、**既存のMavenプロジェクト**&#x200B;を選択します。

![data-source](assets/import-mvn-project.JPG)

「次へ」をクリックします。

「**参照**」ボタンをクリックして、`c:\aemformsbundles\learningaemform`を選択します。

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>必要に応じて、適切なモジュールをインポートするよう選択できます。 プロジェクトでJavaコードの作成のみを行う場合は、コアモジュールのみを選択して読み込みます。

**「完了」**&#x200B;をクリックして、インポート処理を開始します。

プロジェクトがEclipseに読み込まれ、多数の`learningaemforms.xxxx`フォルダーが表示されます

`learningaemforms.core`フォルダーの下の`src/main/java`を展開します。 これは、ほとんどのコードを書き込むフォルダーです。

![data-source](assets/learning-core.JPG)

## プロジェクトを構築する

OSGiサービスまたはサーブレットを作成したら、Felix Webコンソールを使用してデプロイできるOSGiバンドルを生成するために、プロジェクトを構築する必要があります。 [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/)を参照して、適切なクライアントSDKをMavenプロジェクトに含めてください。 以下に示すように、コアプロジェクトの`pom.xml`のdependenciesセクションにAEM FD Client SDKを含める必要があります。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

プロジェクトをビルドするには、次の手順に従います。

* **コマンドプロンプトウィンドウ**&#x200B;を開きます。
* `c:\aemformsbundles\learningaemforms\core` に移動します。
* コマンド`mvn clean install`を実行します。
すべて正常に動作する場合は、次の場所`C:\AEMFormsBundles\learningaemforms\core\target`にバンドルが表示されます。 これで、このバンドルをFelix Webコンソールを使用してAEMにデプロイする準備が整いました。
