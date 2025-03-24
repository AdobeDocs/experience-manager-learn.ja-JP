---
title: カスタムドメイン名のオプション
description: AEM as a Cloud Service でホストされる web サイトのカスタムドメイン名を管理および実装する方法について説明します。
version: Experience Manager as a Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
exl-id: e11ff38c-e823-4631-a5b0-976c2d11353e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 95%

---

# カスタムドメイン名のオプション

AEM as a Cloud Service でホストされる web サイトのドメイン名を管理および実装する方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## 事前準備

カスタムドメイン名の実装を開始する前に、次の概念を理解しておく必要があります。

### ドメイン名とは

ドメイン名は、adobe.comのような人間にわかりやすい名前の web サイト名で、インターネット上の特定の場所（170.2.14.16 のような IP アドレス）を指します。

### AEM as a Cloud Service のデフォルトのドメイン名

デフォルトでは、AEM as a Cloud Service には、`*.adobeaemcloud.com` で終わるデフォルトのドメイン名がプロビジョニングされます。`*.adobeaemcloud.com` に対して発行されたワイルドカード SSL 証明書はすべての環境に自動的に適用され、このワイルドカード証明書はアドビの責任となります。

デフォルトのドメイン名は `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com` 形式です。

- `<SERVICE-TYPE>` は、**作成者**、**公開**&#x200B;または&#x200B;**プレビュー**&#x200B;になります。
- `<PROGRAM-ID>` は、プログラムの一意の ID です。1 つの組織に複数のプログラムを含めることができます。
- `<ENVIRONMENT-ID>` は環境の一意の識別子で、各プログラムには&#x200B;**迅速な開発（RDE）**、**開発**、**ステージ**、**実稼動**&#x200B;の 4 つの環境が含まれます。各環境には、プレビュー環境が含まれない **RDE** を除き、上記の 3 つのサービスタイプが含まれます。

要約すると、すべての AEM as a Cloud Service 環境をプロビジョニングすると、デフォルトのドメイン名と組み合わされた **11** 個（RDE にはプレビュー環境が含まれない）の一意の URL が作成されます。

### アドビが管理する CDN と顧客が管理する CDN

待ち時間を短縮し、web サイトのパフォーマンスを向上させるために、AEM as a Cloud Service はアドビが管理するコンテンツ配信ネットワーク（CDN）と統合されています。アドビが管理する CDN は、すべての環境で自動的に有効になります。詳しくは、[AEM as a Cloud Service のキャッシュ](../caching/overview.md)を参照してください。

ただし、お客様は、**顧客が管理する CDN** と呼ばれる独自の CDN を使用することもできます。必須ではありませんが、企業のポリシーやその他の理由により、これを使用するお客様は少数です。この場合、CDN の設定を管理するのはお客様の責任となります。

### カスタムドメイン名

ブランディング、信頼性、ビジネス開発の目的から、カスタムドメイン名は常にデフォルトのドメイン名よりも優先されます。ただし、**公開**&#x200B;および&#x200B;**プレビュー**&#x200B;のサービスタイプにのみ適用でき、**作成者**&#x200B;には適用できません。

カスタムドメイン名を追加する際は、指定したカスタムドメインに対して有効な SSL 証明書を指定する必要があります。SSL 証明書は、信頼済みの証明機関（CA）によって署名されている有効な証明書である必要があります。

通常、お客様は、実稼動環境（AEM as a Cloud Service web サイト）にカスタムドメイン名を使用し、**ステージ**&#x200B;や&#x200B;**開発**&#x200B;などの下位環境にカスタムドメイン名を使用する場合もあります。

| AEM サービスタイプ | サポートされているカスタムドメイン |
|---------------------|:-----------------------:|
| 作成者 | ✘ |
| プレビュー | ✔ |
| 公開 | ✔ |

## ドメイン名の実装

アドビが管理する CDN または顧客が管理する CDN を使用してドメイン名を実装するには、次のフローチャートに従ってプロセスを進めます。

![ドメイン名管理フローチャート](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

また、次の表に、特定の設定を管理する場所を示します。

| 使用するカスタムドメイン名 | SSL 証明書の追加先 | ドメイン名の追加先 | DNS レコードの設定場所 | HTTP ヘッダー検証 CDN ルールは必要ですか？ |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| アドビが管理する CDN | Adobe Cloud Manager | Adobe Cloud Manager | DNS ホスティングサービス | ✘ |
| 顧客が管理する CDN | CDN ベンダー | CDN ベンダー | DNS ホスティングサービス | ✔ |

### ステップバイステップチュートリアル

ドメイン名の管理プロセスを理解したら、次のチュートリアルに従って、AEM as a Cloud Service web サイトにカスタムドメイン名を実装できます。

**[アドビが管理する CDN を使用したカスタムドメイン名](./custom-domain-name-with-adobe-managed-cdn.md)**：このチュートリアルでは、**アドビが管理する CDN を使用して AEM as a Cloud Service web サイト**にカスタムドメイン名を追加する方法について説明します。
**[顧客が管理する CDN を使用したカスタムドメイン名](./custom-domain-names-with-customer-managed-cdn.md)**：このチュートリアルでは、**顧客が管理する CDN を使用して AEM as a Cloud Service web サイト**&#x200B;にカスタムドメイン名を追加する方法について説明します。
