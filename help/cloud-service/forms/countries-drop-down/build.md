---
title: 国コンポーネントの構築、デプロイ、テスト
description: ビルド、デプロイ、テスト
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
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 2%

---

# 国コンポーネントの構築、デプロイ、テスト

すべてのモジュールをビルドし、`all` パッケージをAEMのローカルインスタンスにデプロイするには、プロジェクトのルートディレクトリで次のコマンドを実行します。

```mvn clean install -PautoInstallSinglePackage```

## コンポーネントのテスト

国コンポーネントをAEM Forms Cloud Ready インスタンスに統合して設定するには、次の手順に従います。

* [countries](assets/countries.zip) zip ファイルの内容を解凍します。 各ファイルには、特定の大陸のデータが含まれている必要があります。
* content/dam/corecomponent.Thisの下に json ファイルをアップロードします。このファイルはコードによって検索される場所です。JSON ファイルを別の場所に保存する場合は、CountriesDropDownImpl クラスの Java コードを更新する必要があります。 特に、JSON ファイルが読み込まれる init （） メソッドのパスを更新します。例えば、JSON ファイルを content/dam/mydata/に保存する場合は、次のようにパスを更新します。

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* AEM Forms Cloud Ready インスタンスにログインします
* アダプティブフォームを作成し、国コンポーネントをフォームにドロップします
* ダイアログエディターを使用して国コンポーネントを設定し、大陸など様々なプロパティを設定します
  ![ 続く ](assets/select-continent.png)
* フォームをプレビューし、国ドロップダウンが期待どおりに動作することを確認します

