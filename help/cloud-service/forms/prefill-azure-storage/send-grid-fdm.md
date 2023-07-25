---
title: SendGrid を使用して電子メールを送信
description: トリガー済みフォームへのリンクを含む電子メールを保存する
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
kt: 13717
source-git-commit: 52c8d96a03b4d6e4f2a0a3c92f4307203e236417
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 15%

---

# SendGrid との統合

AEM Formsのデータ統合を使用すると、様々なデータソースを設定し、AEM Formsと接続することができます。 データ統合機能には、接続されたデータソース全体で、ビジネスエンティティとサービスの統一されたデータ表現スキーマを作成するための直感的なユーザーインターフェイスが用意されています。

SendGrid API を使用して、動的テンプレートを使用した電子メールを送信しました。 動的テンプレートを使用して電子メールを送信する際の SendGrid API に精通していることを前提としています。 API に説明する Swagger ファイルは、このチュートリアルの一部として提供されています。

## 統合の作成

AEM Formsと SendGrid の統合を作成するには、次の手順に従ってください

* を使用して RESTful データソースを作成する [swagger ファイル](./assets/SendGridWithDynamicTemplate.yaml). [詳しい手順については、このビデオを参照してください](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja) AEM Formsでのデータソースの作成時
* 前の手順で作成したデータソースに基づいて、フォームデータモデルを作成します。[詳細なドキュメントに従います](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) フォームデータモデルを作成する際に使用します。

このチュートリアルで作成したフォームデータモデルは、記事のアセットの一部として含まれています。

### 次の手順

[Azure ストレージ統合の作成](./create-fdm.md)


