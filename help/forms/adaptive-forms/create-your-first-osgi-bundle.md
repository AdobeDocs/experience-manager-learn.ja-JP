---
title: AEMフォームでの最初のOSGiバンドルの作成
description: mavenとeclipseを使用した最初のOSGiバンドルの構築
feature: administration
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 48060b4d8c4b502e0c099ae8081695f97b423037
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 2%

---


# 最初のOSGiバンドルの作成

OSGiバンドルは、Javaコード、リソース、およびバンドルとその依存関係を説明するマニフェストを含むJava™アーカイブファイルです。 バンドルは、アプリケーションのデプロイメントの単位です。 この記事は、OSGiサービスまたはサーブレットをAEM Forms6.4または6.5を使用して作成する開発者を対象としています。最初のOSGiバンドルを作成するには、次の手順に従ってください。


## JDK のインストール

サポートされているバージョンのJDKをインストールします。 JDK1.8を使用しました。環境変数に&#x200B;**JAVA_HOME**が追加されていて、JDKのインストール先のルートフォルダーを指していることを確認してください。
パ追加スの%JAVA_HOME%/bin

![データソース](assets/java-home.JPG)

>[!NOTE]
> JDK 15は使用しないでください。 AEMではサポートされていません。

### JDKバージョンのテスト

新しいコマンドプロンプトウィンドウを開き、次のように入力します。`java -version`. `JAVA_HOME`変数で識別されるJDKのバージョンを取得する必要があります

![データソース](assets/java-version.JPG)

## Mavenのインストール

Mavenは、主にJavaプロジェクトで使用されるビルド自動化ツールです。 ローカルシステムにmavenをインストールするには、次の手順に従ってください。

* Cドライブに`maven`という名前のフォルダーを作成します
* [バイナリzipアーカイブ](http://maven.apache.org/download.cgi)をダウンロードします。
* zipアーカイブの内容を`c:\maven`に抽出します。
* `M2_HOME`という名前の環境変数を作成し、値を`C:\maven\apache-maven-3.6.0`にします。 私の場合、**mvn**&#x200B;のバージョンは3.6.0です。この記事を書く時点では、最新のmavenバージョンは3.6.3です。
* パス追加の`%M2_HOME%\bin`
* 変更を保存する
* 新しいコマンドプロンプトを開き、`mvn -version`と入力します。 下のスクリーンショットに示すように、**mvn**&#x200B;のバージョンが表示されます

![データソース](assets/mvn-version.JPG)

## Settings.xml

Maven `settings.xml`ファイルは、Mavenの実行を設定する値を様々な方法で定義します。 最も一般的に使用されるのは、ローカルリポジトリの場所、代替リモートリポジトリサーバー、およびプライベートリポジトリ用の認証情報を定義する場合です。

`C:\Users\<username>\.m2 folder`に移動
[settings.zip](assets/settings.zip)ファイルの内容を抽出し、`.m2`フォルダーに配置します。

## Eclipse のインストール

[eclipse](https://www.eclipse.org/downloads/)の最新バージョンをインストールします。

## 最初のプロジェクトの作成

アーキタイプは、Mavenプロジェクトテンプレートツールキットです。 アーキタイプは、同じ種類の他のすべての要素を作成する元のパターンまたはモデルとして定義されます。 Mavenプロジェクトを生成する一貫性のある方法を提供するシステムを提供しようとしているので、この名前に合います。 アーキタイプは、ユーザー用のMavenプロジェクトテンプレートを作成する際に役立ち、ユーザーはこれらのプロジェクトテンプレートのパラメータ化バージョンを生成する手段を提供します。
最初のMavenプロジェクトを作成するには、次の手順に従います。

* Cドライブに`aemformsbundles`という名前の新しいフォルダーを作成します
* コマンドプロンプトを開き、`c:\aemformsbundles`に移動します
* コマンドプロンプトで次のコマンドを実行します
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Mavenプロジェクトはインタラクティブに生成され、

| プロパティ名 | 有意性 | 値 |
------------------------|---------------------------------------|---------------------
| groupId | groupIdは、すべてのプロジェクトでプロジェクトを一意に識別します | com.learningaemforms.adobe |
| appsFolderName | プロジェクト構造を保持するフォルダの名前 | learningforms |
| artifactId | artifactIdは、バージョンのないjarの名前です。 作成した場合は、小文字で不思議な記号を使用せず、任意の名前を選択できます。 | learningforms |
| version | 配布する場合は、数値とドット（1.0、1.1、1.0.1など）を含む標準的なバージョンを選択できます。 | 1.0 |

Enterキーを押して、その他のプロパティのデフォルト値を受け入れます。
うまくいけば、コマンドウィンドウにビルド成功のメッセージが表示されます。

## MavenプロジェクトからのEclipseプロジェクトの作成

作業ディレクトリを`learningaemforms`に変更します。
コマンドラインから`mvn eclipse:eclipse`を実行中
上記のコマンドは、pomファイルを読み取り、正しいメタデータを持つEclipseプロジェクトを作成して、Eclipseがプロジェクトのタイプ、関係、クラスパスなどを認識できるようにします。

## プロジェクトをEclipseにインポート

**Eclipse**&#x200B;を起動

**ファイル —>読み込み**&#x200B;に移動し、次に示すように&#x200B;**既存のMavenプロジェクト**&#x200B;を選択します

![データソース](assets/import-mvn-project.JPG)

「次へ」をクリック

「**参照**」ボタンをクリックして`c:\aemformsbundles\learningaemform`を選択します

![データソース](assets/select-mvn-project.JPG)

>[!NOTE]
>必要に応じて、適切なモジュールを読み込むよう選択できます。 プロジェクトでJavaコードを作成する場合のみ、コアモジュールを選択して読み込みます。

「**完了**」をクリックして、インポート処理を開始します

プロジェクトがEclipseにインポートされると、多数の`learningaemforms.xxxx`フォルダーが表示されます

`learningaemforms.core`フォルダーの下の`src/main/java`を展開します。 これは、ほとんどのコードを書くフォルダーです。

![データソース](assets/learning-core.JPG)

## プロジェクトを構築する

OSGiサービスまたはサーブレットを記述したら、プロジェクトを構築して、Felix Webコンソールを使用してデプロイできるOSGiバンドルを生成する必要があります。 [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/)を参照して、適切なクライアントSDKをMavenプロジェクトに含めてください。 以下に示すように、コアプロジェクトの`pom.xml`の依存関係セクションにAEM FDクライアントSDKを含める必要があります。

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

プロジェクトをビルドするには、次の手順に従います。

* **コマンドプロンプトウィンドウ**&#x200B;を開きます
* `c:\aemformsbundles\learningaemforms\core` に移動します。
* コマンド`mvn clean install`を実行します
問題が解決したら、次の場所`C:\AEMFormsBundles\learningaemforms\core\target`にバンドルが表示されます。 これで、Felix Webコンソールを使用して、このバンドルをAEMにデプロイする準備が整いました。
