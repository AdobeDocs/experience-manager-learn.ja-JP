---
title: AEM Forms と SendGrid の統合
description: AEM Forms を使用して、SengGrid クラウドベースのメール配信プラットフォームを活用します。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 100%

---

# AEM Forms と SendGrid の統合

このテクニカルガイドでは、AEM Forms から SendGrid 動的テンプレートを使用してメールを送信するプロセスについて説明します。このガイドでは、動的テンプレートを活用してメールコンテンツを効果的にパーソナライズする方法を明確に理解できるようにすることを目的としています。

動的テンプレートを使用すると、アダプティブフォームで取得したデータに基づいて受信者に様々なコンテンツを表示できるメールテンプレートを作成できます。パーソナライゼーション変数を利用することで、ターゲットを絞り込んでカスタマイズされたメールエクスペリエンスを提供し、オーディエンスの共感を得ることができます。

さらに、Swagger ファイルの使用方法について詳しく説明します。これにより、顧客の名前とメールアドレスを含めたり、適切な動的メールテンプレートを選択したりすることで、メールをさらにパーソナライズできます。

このドキュメントのステップバイステップの手順に従って、SendGrid 動的テンプレートと AEM Forms の機能を活用し、メールでの通信を新たなレベルのエンゲージメントと関連性へと高めます。それでは、始めましょう。

## 前提条件

AEM Forms から SendGrid 動的テンプレートを使用してメールを送信する前に、次の前提条件を満たしている必要があります。

1. **SendGrid アカウント**：[https://sendgrid.com](https://sendgrid.com) で SendGrid アカウントに新規登録して、メール配信サービスにアクセスする。SendGrid を AEM Forms と統合するには、アカウント資格情報が必要です。
1. **データソースの作成に関する知識**：AEM Forms でのデータソースの作成に関する実用的な知識がある。必要に応じて、[データソースの作成](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html?lang=ja)に関するドキュメントを参照して、詳細な手順を確認してください。
1. **フォームデータモデルに関する知識**：AEM Forms のフォームデータモデルの概念を理解している。必要に応じて、[フォームデータモデルの作成](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=ja)に関するドキュメントを参照して、必要な理解を確認してください。

これらの前提条件を満たすことで、AEM Forms から SendGrid 動的テンプレートを使用して、メールを効果的に送信するための重要な知識とリソースが得られます。

## サンプルアセット

この記事で提供されるサンプルアセットには、次のものが含まれます。

* **[Swagger ファイル](assets/SendGridWithDynamicTemplate.yaml)**：このファイルを使用すると、動的メールテンプレートを使用してメールを送信できます。必要な仕様と設定を指定することで、SendGrid および AEM Forms と統合してシームレスなメール配信を実現できます。

提供される Swagger ファイルは、動的テンプレートを使用してメール機能を実装するための参照または出発点として自由に利用できます。

## テストの手順

このガイドで説明する機能をテストするには、次の手順に従います。

1. アセットフォルダーにある [Swagger ファイル](assets/SendGridWithDynamicTemplate.yaml)をダウンロードします。
2. ダウンロードした Swagger ファイルと SendGrid 資格情報を使用して、Restful データソースを作成します。
3. Restful データソースに基づいてフォームデータモデルを作成します。
4. 要件に応じて、フォームデータモデルの `mail/send` POST 操作を呼び出します。例えば、ボタンのクリック時にメールをトリガーしたり、AEM Forms Workflow の一部としてメールを含めたりできます。

サービスのサンプルペイロードを以下に示します。プレースホルダーの値を独自のデータに置き換えます。

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

`template_id` が SendGrid 動的メールテンプレートの ID に対応していることと、メールアドレスが有効で、SendGrid によって検証されていることを確認します。`personalizations` セクションの値を使用すると、アダプティブフォームからユーザーが入力したデータを使用してメールをパーソナライズできます。

これらの手順に従って、提供されたペイロードをカスタマイズすると、SendGrid 動的テンプレートと AEM Forms の統合を効果的にテストできます。
