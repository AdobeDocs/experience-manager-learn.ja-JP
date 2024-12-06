---
title: 国コンポーネントの作成、デプロイ、テスト
description: 作成、デプロイ、テスト
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: ht
source-wordcount: '209'
ht-degree: 100%

---

# 国コンポーネントの作成、デプロイ、テスト

すべてのモジュールを作成し、`all` パッケージを AEM のローカルインスタンスにデプロイするには、プロジェクトルートディレクトリで次のコマンドを実行します。

```mvn clean install -PautoInstallSinglePackage```

## コンポーネントのテスト

国コンポーネントを AEM Forms クラウド対応インスタンスに統合および設定するには、次の手順に従います。

* [国](assets/countries.zip)の zip ファイルのコンテンツを抽出します。各ファイルには、特定の大陸のデータが含まれています。
* content/dam/corecomponent の下に JSON ファイルをアップロードします。これは、コードが JSON ファイルを検索する場所です。JSON ファイルを別の場所に保存する場合は、CountriesDropDownImpl クラスの Java コードを更新する必要があります。具体的には、JSON ファイルが読み込まれる init() メソッド内のパスを更新します。例えば、JSON ファイルを content/dam/mydata/ に保存する場合は、パスを次のように更新します

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* AEM Forms クラウド対応インスタンスにログインします
* アダプティブフォームを作成し、フォームに国コンポーネントをドロップします
* ダイアログエディターを使用して国コンポーネントを設定し、大陸を含む様々なプロパティを設定します
  ![大陸](assets/select-continent.png)
* フォームをプレビューし、国ドロップダウンが期待どおりに動作することを確認します

