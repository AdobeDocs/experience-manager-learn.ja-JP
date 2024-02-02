---
title: ウェブフックとAEMイベント
description: Webhook でAEMイベントを受け取り、ペイロード、ヘッダー、メタデータなどのイベントの詳細を確認する方法を説明します。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 156
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
source-git-commit: f0930e517254b6353fe50c3bbf9ae915d9ef6ca3
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 0%

---


# ウェブフックとAEMイベント

Webhook でAEMイベントを受け取り、ペイロード、ヘッダー、メタデータなどのイベントの詳細を確認する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3427051?quality=12&learn=on)

この例では、Adobe提供の _ホスト型 webhook_ を使用すると、独自の Webhook を設定しなくてもAEMイベントを受け取ることができます。 このAdobe提供の Webhook は、 [グリッチ](https://glitch.com/):Web アプリケーションの構築とデプロイに役立つ Web ベースの環境を提供することで知られているプラットフォームです。 ただし、独自の Webhook を使用するオプションも使用できます。

## 前提条件

このチュートリアルを完了するには、以下が必要です。

- AEMas a Cloud Service環境 [AEM Eventing enabled](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [AEM Events 用に設定されたAdobe Developer Console プロジェクト](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

>[!IMPORTANT]
>
>AEMas a Cloud Serviceイベントは、プレリリースモードで登録ユーザーのみが使用できます。 AEMのas a Cloud Service環境でAEMイベンティングを有効にするには、 [AEM-Eventing チーム](mailto:grp-aem-events@adobe.com).

## Webhook にアクセス

Adobeが提供する Webhook にアクセスするには、次の手順に従います。

- 次にアクセスできることを確認します： [問題 — ホストされた Webhook](https://lovely-ancient-coaster.glitch.me/) をクリックします。

  ![問題 — ホストされた Webhook](../assets/examples/webhook/glitch-hosted-webhook.png)

- ウェブフックの一意の名前を入力します（例： ）。 `<YOUR_PETS_NAME>-aem-eventing` をクリックします。 **接続**. 次のようになります。 `Connected to: ${YOUR-WEBHOOK-URL}` メッセージが画面に表示されます。

  ![問題 — ウェブフックの作成](../assets/examples/webhook/glitch-create-webhook.png)

- 次の項目をメモします。 **Webhook URL**. このチュートリアルの後半で必要になります。

## Adobe Developer Console Project での Webhook の設定

上記の Webhook URL でAEMイベントを受け取るには、次の手順に従います。

- Adobe Analytics の [Adobe Developer Console](https://developer.adobe.com)をクリックして開き、プロジェクトに移動します。

- の下 **製品とサービス** 「 」セクションで、省略記号をクリックします。 `...` をクリックし、AEMイベントを Webhook に送信する必要がある目的のイベントカードの横にある「 」をクリックし、 **編集**.

  ![Adobe Developer Console プロジェクトの編集](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 新しく開かれた **イベント登録の設定** ダイアログ、クリック **次へ** 先に進む **イベントの受信方法** 手順

  ![Adobe Developer Console プロジェクトの設定](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- Adobe Analytics の **イベントの受信方法** ステップ、選択 **ウェブフック** 」オプションを選択し、 **Webhook URL** Glitch がホストする webhook から先ほどコピーし、 **設定済みイベントを保存**.

  ![Adobe Developer Console Project Webhook](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Glitch Webook ページには、GETリクエストが表示されます。これは、Webhook URL を検証するためにAdobe I/Oイベントによって送信されるチャレンジリクエストです。

  ![問題 — チャレンジリクエスト](../assets/examples/webhook/glitch-challenge-request.png)


## トリガーAEMイベント

上記のAdobe Developer Console プロジェクトに登録されたAEMas a Cloud Service環境からAEMイベントをトリガーするには、次の手順に従います。

- を介してAEM as a Cloud Serviceオーサー環境にアクセスし、ログインします。 [Cloud Manager](https://my.cloudmanager.adobe.com/).

- 次の条件に応じて、 **購読済みイベント**&#x200B;コンテンツフラグメントの作成、更新、削除、公開、非公開を行います。

## イベントの詳細を確認

上記の手順を完了すると、AEMイベントが Webhook に配信されていることがわかります。 Glitch の Webhook ページでPOSTリクエストを探します。

![エラー —POSTリクエスト](../assets/examples/webhook/glitch-post-request.png)

POST要求の主な詳細を次に示します。

- パス： `/webhook/${YOUR-WEBHOOK-URL}`例： `/webhook/AdobeTM-aem-eventing`

- headers：リクエストヘッダー。Adobe I/Oイベントによって送信されます。次に例を示します。

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- body/payload：リクエストイベントによって送信されるAdobe I/O本文。次に例を示します。

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

AEMイベントの詳細に、Webhook でイベントを処理するために必要な情報がすべて含まれていることを確認できます。 例えば、イベントタイプ (`type`)，イベントソース (`source`)，イベント id (`event_id`)，イベント時刻 (`time`)、およびイベントデータ (`data`) をクリックします。

## その他のリソース

- [ウェブフックのソースコードの問題](https://glitch.com/edit/#!/lavery-encient-coaster) は参照可能です。
