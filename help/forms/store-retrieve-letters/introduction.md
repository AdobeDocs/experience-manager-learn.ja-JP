---
title: レターの保存と再開
description: ドラフトレターを保存して取得する方法を説明します。
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: e032070b-7332-4c2f-97ee-7e887a61aa7a
last-substantial-update: 2022-01-07T00:00:00Z
duration: 160
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '119'
ht-degree: 100%

---

# はじめに

インタラクティブ通信を使用すると、アドホック通信を準備するエージェントは、完了した通信の一部を保存し、それと同じ通信を取得して作業を続行できます。AEM Forms は[サービスプロバイダーインターフェイス](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/ccm/ccr/ccrDocumentInstance/api/services/CCRDocumentInstanceService.html)を提供します。お客様は、このインターフェイスを実装して、保存と再開の機能を利用することになっています。

この記事では、MySQL データベースを使用してレターインスタンスのメタデータを保存します。レターデータはファイルシステムに保存されます。

次のビデオでは、このユースケースを示しています。

>[!VIDEO](https://video.tv.adobe.com/v/3441440?quality=12&learn=on&captions=jpn)

## 前提条件

ニーズに合わせてソリューションを実装するには、以下が必要です。

* AEM Forms の使用経験
* AEM Server 6.5 と Forms アドオン
* OSGi バンドルのビルドへの精通
