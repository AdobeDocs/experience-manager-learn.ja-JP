---
title: ジャーナリングとAEMイベント
description: ジャーナルからAEMイベントの初期セットを取得し、各イベントの詳細を調べる方法について説明します。
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 163
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14734
thumbnail: KT-14734.jpeg
source-git-commit: f0930e517254b6353fe50c3bbf9ae915d9ef6ca3
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 0%

---


# ジャーナリングとAEMイベント

ジャーナルからAEMイベントの初期セットを取得し、各イベントの詳細を調べる方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3427052?quality=12&learn=on)

ジャーナル処理は、AEMイベントを使用するプルメソッドで、ジャーナルは、イベントの順序付きリストです。 Adobe I/Oイベントジャーナル API を使用すると、ジャーナルからAEMイベントを取得し、アプリケーションで処理できます。 この方法を使用すると、指定したケイデンスに基づいてイベントを管理し、それらを一括で効率的に処理できます。 詳しくは、 [ジャーナリング](https://developer.adobe.com/events/docs/guides/journaling_intro/) 保持期間やページネーションなどの重要な考慮事項を含む、詳細なインサイトに関する情報。

Adobe Developer Console プロジェクト内では、すべてのイベント登録がジャーナリングに対して自動的に有効になり、シームレスな統合が可能になります。

この例では、Adobe提供の _ホスト Web アプリケーション_ を使用すると、アプリケーションを設定しなくても、ジャーナルからAEM Events の最初のバッチを取得できます。 このAdobe提供 Web アプリケーションは、次の場所でホストされます： [グリッチ](https://glitch.com/):Web アプリケーションの構築とデプロイに役立つ Web ベースの環境を提供することで知られているプラットフォームです。 ただし、独自のアプリケーションを使用するオプションも使用できます。

## 前提条件

このチュートリアルを完了するには、以下が必要です。

- AEMas a Cloud Service環境 [AEM Eventing enabled](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [AEM Events 用に設定されたAdobe Developer Console プロジェクト](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).

>[!IMPORTANT]
>
>AEMas a Cloud Serviceイベントは、プレリリースモードで登録ユーザーのみが使用できます。 AEMのas a Cloud Service環境でAEMイベンティングを有効にするには、 [AEM-Eventing チーム](mailto:grp-aem-events@adobe.com).

## Web アプリケーションにアクセス

Adobeが提供する Web アプリケーションにアクセスするには、次の手順に従います。

- 次にアクセスできることを確認します： [問題 — ホストされた Web アプリケーション](https://indigo-speckle-antler.glitch.me/) をクリックします。

  ![問題 — ホストされた Web アプリケーション](../assets/examples/journaling/glitch-hosted-web-application.png)

## Adobe Developer Console プロジェクトの詳細を収集

ログからAEMイベントを取得するには、次のような資格情報を使用します。 _IMS 組織 ID_, _クライアント ID_、および _アクセストークン_ が必要です。 これらの資格情報を収集するには、次の手順に従います。

- Adobe Analytics の [Adobe Developer Console](https://developer.adobe.com)をクリックして開き、プロジェクトに移動します。

- の下 **資格情報** セクションで、 **OAuth サーバー間通信** 開くためのリンク **資格情報の詳細** タブをクリックします。

- 次をクリック： **アクセストークンを生成** ボタンをクリックしてアクセストークンを生成します。

  ![Adobe Developer Console プロジェクトのアクセストークンの生成](../assets/examples/journaling/adobe-developer-console-project-generate-access-token.png)

- をコピーします。 **生成されたアクセストークン**, **クライアント ID**、および **組織 ID**. このチュートリアルの後半で必要になります。

  ![Adobe Developer Console プロジェクトコピー資格情報](../assets/examples/journaling/adobe-developer-console-project-copy-credentials.png)

- すべてのイベント登録は、ジャーナリングに対して自動的に有効になります。 次の手順で _一意のジャーナル API エンドポイント_ イベント登録の「 AEM Events 」を購読しているイベントカードをクリックします。 次から： **登録の詳細** タブ、コピー **ジャーナリングの一意の API エンドポイント**.

  ![Adobe Developer Console プロジェクトイベントカード](../assets/examples/journaling/adobe-developer-console-project-events-card.png)

## AEM Events ジャーナルを読み込み

話を簡単にするために、このホストされた Web アプリケーションは、ジャーナルからAEMイベントの最初のバッチのみを取得します。 これらは、ジャーナルで利用可能な最も古いイベントです。 詳しくは、 [最初の一連のイベント](https://developer.adobe.com/events/docs/guides/api/journaling_api/#fetching-your-first-batch-of-events-from-the-journal).

- Adobe Analytics の [問題 — ホストされた Web アプリケーション](https://indigo-speckle-antler.glitch.me/)、 **IMS 組織 ID**, **クライアント ID**、および **アクセストークン** 前の手順でAdobe Developer Console プロジェクトからコピーし、「 **送信**.

- 成功すると、テーブルコンポーネントにAEMイベントジャーナルデータが表示されます。

  ![AEM Events ジャーナルデータ](../assets/examples/journaling/load-journal.png)

- イベントペイロード全体を表示するには、行をダブルクリックします。 AEMイベントの詳細に、Webhook でイベントを処理するために必要な情報がすべて含まれていることを確認できます。 例えば、イベントタイプ (`type`)，イベントソース (`source`)，イベント id (`event_id`)，イベント時刻 (`time`)、およびイベントデータ (`data`) をクリックします。

  ![AEM Event Payload を完了](../assets/examples/journaling/complete-journal-data.png)

## その他のリソース

- [ウェブフックのソースコードの問題](https://glitch.com/edit/#!/indigo-speckle-antler) は参照可能です。 これは、 [AdobeReact スペクトル](https://react-spectrum.adobe.com/react-spectrum/index.html) UI をレンダリングするコンポーネント。

- [Adobe I/Oイベントジャーナル API](https://developer.adobe.com/events/docs/guides/api/journaling_api/) は、イベントの最初、次、最後のバッチ、ページネーションなど、API に関する詳細情報を提供します。
