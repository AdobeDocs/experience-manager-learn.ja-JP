---
title: SendGrid を使用したメールの送信
description: 保存されたフォームへのリンクを含んだメールをトリガーします
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: ht
source-wordcount: '197'
ht-degree: 100%

---

# SendGrid との統合

AEM Forms のデータ統合機能により、各種のデータソースを設定し AEM Forms に接続することができます。データ統合機能には、接続されたデータソース全体で、ビジネスエンティティとサービスの統一されたデータ表現スキーマを作成するための直感的なユーザーインターフェイスが用意されています。

SendGrid API を介して、動的テンプレートを使用してメールを送信しました。動的テンプレートを使用してメールを送信するための SendGrid API を熟知していることを前提としています。API に従って記述するための Swagger ファイルが、このチュートリアルの一部として提供されています。

## 統合の作成

AEM Forms と SendGrid の統合を作成するには、次の手順に従ってください。

* [Swagger ファイル](./assets/SendGridWithDynamicTemplate.yaml)を使用して RESTful データソースを作成します。AEM Forms でデータソースを作成する[手順について詳しくは、このビデオをご覧ください](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja)。
* 前の手順で作成したデータソースに基づいて、フォームデータモデルを作成します。フォームデータモデルの作成については、[詳細なドキュメントを参照してください](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html?lang=ja)。

このチュートリアル用に作成したフォームデータモデルは、記事のアセットの一部として含まれています。

### 次の手順

[Azure ストレージ統合の作成](./create-fdm.md)


