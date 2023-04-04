---
title: レターの保存と再開
seo-title: Save and resume letters
description: ドラフトレターを保存して取得する方法を説明します
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 0%

---

# はじめに

インタラクティブ通信を使用すると、アドホック通信を準備するエージェントは、完了した通信の一部を保存し、同じ通信を取得して、作業を続行できます。 AEM Formsが [サービスプロバイダーインターフェイス](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html). お客様は、このインターフェイスを実装して、保存と再開の機能を使用する必要があります。

この記事では、MySQL データベースを使用してレターインスタンスのメタデータを保存します。 レターデータはファイルシステムに保存されます。

次のビデオでは、の使用例を示します。

>[!VIDEO](https://video.tv.adobe.com/v/342129?quality=12&learn=on)

## 前提条件

ニーズに合わせてソリューションを実装するには、以下が必要です

* AEM Formsの使用経験
* AEM Server 6.5 とForms Add On
* OSGi バンドルの構築に精通している必要がある
