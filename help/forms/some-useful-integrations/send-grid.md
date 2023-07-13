---
title: AEM Formsと SendGrid の統合
description: AEM Formsを使用して、SengGrid クラウドベースの E メール配信プラットフォームを活用します。
feature: Adaptive Forms
version: 6.4,6.5
kt: 13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 2%

---

# AEM Formsと SendGrid の統合

このテクニカルガイドでは、AEM Formsから SendGrid 動的テンプレートを使用して E メールを送信するプロセスについて説明します。 このガイドでは、動的テンプレートを活用して E メールコンテンツを効果的にパーソナライズする方法を明確に理解できるようにすることを目的としています。

動的テンプレートを使用すると、アダプティブフォームで取り込まれたデータに基づいて、受信者に対して様々なコンテンツを表示できる電子メールテンプレートを作成できます。 パーソナライゼーション変数を利用することで、オーディエンスの共感を呼ぶ、ターゲットを絞り込んでカスタマイズされた E メールエクスペリエンスを提供できます。

さらに、Swagger ファイルの使用方法を詳しく説明します。Swagger ファイルを使用すると、顧客の名前と E メールアドレスを含め、適切な動的 E メールテンプレートを選択することで、E メールをさらにパーソナライズできます。

このドキュメントの詳しい手順に従って、SendGrid の動的テンプレートとAEM Formsの機能を活用し、メール通信を新しいレベルのエンゲージメントと関連性に高めます。 始めましょう！

## 前提条件

AEM Formsの SendGrid 動的テンプレートを使用した電子メールの送信を続行する前に、次の前提条件を満たしていることを確認してください。

1. **SendGrid アカウント**:次の場所に SendGrid アカウントを登録 [https://sendgrid.com](https://sendgrid.com) をクリックして、e メール配信サービスにアクセスします。 SendGrid をAEM Formsと統合するには、アカウントの資格情報が必要です。
1. **データソースの作成に関する知識**:AEM Formsでのデータソースの作成に関する実務知識がある。 必要に応じて、 [データソースの作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja) を参照してください。
1. **フォームデータモデルに関する知識**:AEM Formsでのフォームデータモデルの概念を理解します。 必要に応じて、 [フォームデータモデルの作成](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ja) 必要な理解を得るために

これらの前提条件を満たすことで、AEM Formsの SendGrid 動的テンプレートを使用して E メールを効果的に送信するための重要な知識とリソースが得られます。

## サンプルアセット

この記事で提供されるサンプルアセットには、以下が含まれます。

* **[Swagger ファイル](assets/SendGridWithDynamicTemplate.yaml)**:このファイルを使用すると、動的な E メールテンプレートを使用して E メールを送信できます。 シームレスな E メール配信を実現するために、SendGrid やAEM Formsと統合するために必要な仕様と設定を提供します。

提供された Swagger ファイルを、動的テンプレートを使用した電子メール機能の実装の参考または出発点として自由に利用できます。

## テストの手順

このガイドで説明する機能をテストするには、次の手順に従います。

1. をダウンロードします。 [swagger ファイル](assets/SendGridWithDynamicTemplate.yaml) assets フォルダーに指定されます。
2. ダウンロードした Swagger ファイルと SendGrid の資格情報を使用して、Restful データソースを作成します。
3. Restful データソースに基づいてフォームデータモデルを作成します。
4. を呼び出す `mail/send` 必要に応じて、フォームデータモデルのPOST操作を行います。 例えば、「クリック時に電子メールをトリガー」ボタンをクリックしたり、AEM Formsワークフローの一部に含めたりできます。

このサービスのサンプルペイロードは次のとおりです。 プレースホルダー値を独自のデータに置き換えます。

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

次を確認します。 `template_id` は、SendGrid 動的電子メールテンプレートの ID に対応し、電子メールアドレスは SendGrid で有効で検証されます。 の値 `personalizations` 「 」セクションでは、アダプティブフォームのユーザー入力データを使用して e メールをパーソナライズできます。

以下の手順に従い、提供されたペイロードをカスタマイズすることで、SendGrid 動的テンプレートとAEM Formsの統合を効果的にテストできます。

