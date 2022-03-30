---
title: AEM Formsでの最初の OSGi バンドルの作成
description: Maven と Eclipse を使用して最初の OSGi バンドルを構築
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '835'
ht-degree: 2%

---

# 最初の OSGi バンドルを作成します。

OSGi バンドルは、Java コード、リソース、およびバンドルとその依存関係を記述するマニフェストを含む Java™アーカイブファイルです。 バンドルは、アプリケーションのデプロイメント単位です。 この記事は、AEM Forms 6.4 または 6.5 を使用して OSGi サービスまたはサーブレットを作成する開発者向けです。最初の OSGi バンドルを構築するには、次の手順に従います。


## JDK のインストール

サポートされているバージョンの JDK をインストールします。 JDK1.8 を使用しています。 **JAVA_HOME** を設定し、JDK インストールのルートフォルダーを指している必要があります。
%JAVA_HOME%/bin をパスに追加

![data-source](assets/java-home.JPG)

>[!NOTE]
> JDK 15 は使用しないでください。 AEMではサポートされていません。

### JDK バージョンのテスト

新しいコマンドプロンプトウィンドウを開き、次のように入力します。 `java -version`. で識別されている JDK のバージョンを取り戻す必要があります。 `JAVA_HOME` 変数

![data-source](assets/java-version.JPG)

## Maven のインストール

Maven は、主に Java プロジェクトで使用されるビルド自動化ツールです。 次の手順に従って、ローカルシステムに maven をインストールしてください。

* という名前のフォルダーを作成します。 `maven` C ドライブ内
* をダウンロードします。 [バイナリ zip アーカイブ](https://maven.apache.org/download.cgi)
* zip アーカイブの内容を次に展開します。 `c:\maven`
* という名前の環境変数を作成します。 `M2_HOME` 値を `C:\maven\apache-maven-3.6.0`. 私の場合、 **mvn** version is 3.6.0.この記事の作成時点での最新の maven バージョンは 3.6.3 です。
* を `%M2_HOME%\bin` を、
* 変更を保存します
* 新しいコマンドプロンプトを開き、次のように入力します。 `mvn -version`. 次のように表示されます。 **mvn** 次のスクリーンショットに示すバージョン

![data-source](assets/mvn-version.JPG)

## Settings.xml

Maven `settings.xml` ファイルは、様々な方法で Maven の実行を設定する値を定義します。 最も一般的に、ローカルリポジトリの場所、代替のリモートリポジトリサーバー、およびプライベートリポジトリの認証情報を定義するために使用されます。

に移動します。 `C:\Users\<username>\.m2 folder`
コンテンツを抽出 [settings.zip](assets/settings.zip) ファイルを作成し、 `.m2` フォルダー。

## Eclipse のインストール

最新バージョンのをインストールする [eclipse](https://www.eclipse.org/downloads/)

## 最初のプロジェクトを作成

アーキタイプは、Maven プロジェクトテンプレートツールキットです。 アーキタイプは、同じ種類の他のすべてのものが作成される元のパターンまたはモデルとして定義されます。 Maven プロジェクトを生成する一貫した方法を提供するシステムを提供しようとするときに、名前が適切になります。 アーキタイプは、作成者がユーザー用の Maven プロジェクトテンプレートを作成するのに役立ち、ユーザーは、これらのプロジェクトテンプレートのパラメーター化されたバージョンを生成する手段を提供します。
最初の Maven プロジェクトを作成するには、次の手順に従います。

* という名前の新しいフォルダーを作成します。 `aemformsbundles` C ドライブ内
* コマンドプロンプトを開き、に移動します。 `c:\aemformsbundles`
* コマンドプロンプトで次のコマンドを実行します。
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Maven プロジェクトはインタラクティブに生成され、次のような多数のプロパティに値を指定するよう求められます。

| プロパティ名 | 有意性 | 値 |
------------------------|---------------------------------------|---------------------
| groupId | groupId は、すべてのプロジェクトをまたいでプロジェクトを一意に識別します | com.learningaemforms.adobe |
| appsFolderName | プロジェクト構造を保持するフォルダーの名前 | learningforms |
| artifactId | artifactId は、バージョンのない jar の名前です。 作成した場合は、任意の名前を小文字で選択し、変な記号は使用できません。 | learningforms |
| version | 配布する場合は、数字とドット付きの一般的なバージョン（1.0、1.1、1.0.1 など）を選択できます。 | 1.0 |

Enter キーを押して、他のプロパティのデフォルト値をそのまま使用します。
すべてが正常に動作する場合は、コマンドウィンドウにビルド成功メッセージが表示されます

## Maven プロジェクトから Eclipse プロジェクトを作成する

作業ディレクトリをに変更します。 `learningaemforms`.
実行 `mvn eclipse:eclipse` 上記のコマンドは、pom ファイルを読み取り、正しいメタデータを使用して Eclipse プロジェクトを作成し、Eclipse がプロジェクトのタイプ、関係、クラスパスなどを認識できるようにします。

## プロジェクトを Eclipse に読み込む

起動 **Eclipse**

に移動します。 **ファイル —> インポート** を選択し、 **既存の Maven プロジェクト** ここに示すように

![data-source](assets/import-mvn-project.JPG)

次へをクリック

を選択します。 `c:\aemformsbundles\learningaemform`をクリックし、 **参照** ボタン

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>必要に応じて、適切なモジュールをインポートするように選択できます。 プロジェクトで Java コードを作成するだけの場合は、コアモジュールのみを選択して読み込みます。

クリック **完了** インポート処理を開始するには

プロジェクトが Eclipse に読み込まれると、 `learningaemforms.xxxx` フォルダー

を展開します。 `src/main/java` の下に `learningaemforms.core` フォルダー。 これは、ほとんどのコードを書き込むフォルダーです。

![data-source](assets/learning-core.JPG)

## プロジェクトを構築する




OSGi サービスまたはサーブレットを作成したら、Felix Web コンソールを使用してデプロイできる OSGi バンドルを生成するために、プロジェクトを構築する必要があります。 詳しくは、 [AEMFD Client SDK](https://search.maven.org/artifact/com.adobe.aemfd/aemfd-client-sdk) を追加して、Maven プロジェクトに適切なクライアント SDK を含めます。 AEM FD Client SDK をの dependencies セクションに含める必要があります。 `pom.xml` を参照してください。







```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

プロジェクトを構築するには、次の手順に従います。

* 開く **コマンドプロンプトウィンドウ**
* `c:\aemformsbundles\learningaemforms\core` に移動します。
* コマンドを実行します。 `mvn clean install -PautoInstallBundle`
上記のコマンドは、で実行されているAEMサーバーにバンドルをビルドおよびインストールします。 `http://localhost:4502`. バンドルは、ファイルシステムの
   `C:\AEMFormsBundles\learningaemforms\core\target` を使用してデプロイできます。 [Felix Web コンソール](http://localhost:4502/system/console/bundles)
