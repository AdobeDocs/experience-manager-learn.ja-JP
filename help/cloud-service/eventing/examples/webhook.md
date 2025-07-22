---
title: Web フックと AEM イベント
description: Web フックで AEM イベントを受け取り、イベントの詳細（ペイロード、ヘッダー、メタデータなど）を確認する方法を学びます。
version: Experience Manager as a Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: e01eb7ff050321a70d84f8a613705799017dbf5d
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 84%

---

# Web フックと AEM イベント

Web フックで AEM イベントを受け取り、イベントの詳細（ペイロード、ヘッダー、メタデータなど）を確認する方法を学びます。


>[!VIDEO](https://video.tv.adobe.com/v/3427051?quality=12&learn=on)


>[!IMPORTANT]
>
>このチュートリアルのライブデモエンドポイントは、以前 [Glitch](https://glitch.com/) でホストされていました。 2025 年 7 月をもって、Glitch はホスティングサービスを廃止し、エンドポイントにアクセスできなくなりました。
>>アドビでは、デモを代替プラットフォームに移行する取り組みを積極的に進めています。 チュートリアルのコンテンツは引き続き正確で、更新されたリンクはすぐに提供されます。
>>ご理解とご辛抱をお願いいたします。

ライブデモエンドポイントが再び使用可能になるまで、独自の Webhook を使用します。

## 前提条件

このチュートリアルを完了するには、次が必要になります。

- [AEM イベント処理が有効](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment)になっている AEM as a Cloud Service環境。

- [AEM イベント用に設定された Adobe Developer Console プロジェクト](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console)。


## Web フックにアクセス

アドビが提供する web フックにアクセスするには、次の手順に従います。

- 新しいブラウザータブで、[Glitch：ホストされた web フック](https://lovely-ancient-coaster.glitch.me/)にアクセスできることを確認します。

  ![Glitch：ホストされた web フック](../assets/examples/webhook/glitch-hosted-webhook.png)

- Web フックに一意の名前（例：`<YOUR_PETS_NAME>-aem-eventing`）を入力し、「**Connect**」をクリックします。「`Connected to: ${YOUR-WEBHOOK-URL}`」というメッセージが画面に表示されます。

  ![Glitch：web フックの作成](../assets/examples/webhook/glitch-create-webhook.png)

- **Web フック URL** をメモします。この情報は、このチュートリアルの後半で必要になります。

## Adobe Developer Console プロジェクトでの web フックの設定

上記の web フック URL で AEM イベントを受け取るには、次の手順に従います。

- [Adobe Developer Console](https://developer.adobe.com) でプロジェクトに移動し、クリックして開きます。

- 「**製品とサービス**」セクションで、AEM イベントを web フックに送信するイベントカードの横にある省略記号 `...` をクリックして、「**編集**」を選択します。

  ![Adobe Developer Console プロジェクトの編集](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- 新しく開かれた&#x200B;**イベント登録の設定**&#x200B;ダイアログで、「**次へ**」をクリックして&#x200B;**イベントの受信方法**&#x200B;の手順に進みます。

  ![Adobe Developer Console プロジェクトの設定](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- **イベントの受信方法**&#x200B;のステップで、「**Web フック**」オプションを選択し、先ほどの Glitch のホストされた web フックからコピーした **web フック URL** を貼り付けて、「**設定済みイベントを保存**」をクリックします。

  ![Adobe Developer Console プロジェクトの web フック](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Glitch の web フックページには、GET リクエストが表示されます。これは、web フック URL を検証するために Adobe I/O イベントによって送信されるチャレンジリクエストです。

  ![Glitch：チャレンジリクエスト](../assets/examples/webhook/glitch-challenge-request.png)


## AEM イベントをトリガー

上記の Adobe Developer Console プロジェクトで登録した AEM as a Cloud Service 環境から AEM イベントをトリガーするには、次の手順に従います。

- [Cloud Manager](https://my.cloudmanager.adobe.com/) から AEM as a Cloud Service オーサー環境にアクセスし、ログインします。

- **購読しているイベント**&#x200B;に応じて、コンテンツフラグメントの作成、更新、削除、公開、非公開を行います。

## イベントの詳細を確認

上記の手順を完了すると、AEM イベントが web フックに配信されているのを確認できます。Glitch の web フックページで POST リクエストを探します。

![Glitch： POST リクエスト](../assets/examples/webhook/glitch-post-request.png)

POST リクエストの主な詳細を次に示します。

- path：`/webhook/${YOUR-WEBHOOK-URL}`（例：`/webhook/AdobeTM-aem-eventing`）

- headers：Adobe I/O イベントによって送信されるリクエストヘッダー。次に例を示します。

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

- 本文／ペイロード：Adobe I/O Events から送信されるリクエスト本文。次に例を示します。

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

Web フックでイベントを処理するために必要な情報がすべて AEM イベントの詳細に含まれていることがわかります。例えば、イベントタイプ（`type`）、イベントソース（`source`）、イベント ID（`event_id`）、イベント時刻（`time`）およびイベントデータ（`data`）などです。

## その他のリソース

- [AEM - イベント Webhook](../assets/examples/webhook/aemeventing-webhook.tgz) のソースコードは、参照用に利用できます。
