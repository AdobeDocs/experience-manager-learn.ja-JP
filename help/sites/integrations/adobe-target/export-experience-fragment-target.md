---
title: Adobe Target へのエクスペリエンスフラグメントの書き出し
description: AEM エクスペリエンスフラグメントを Adobe Target オファーとして公開し書き出す方法について説明します。
feature: Experience Fragments
version: Experience Manager as a Cloud Service
jira: KT-6350
thumbnail: 41245.jpg
topic: Integrations
role: User
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 2c01cda8-f72f-47f7-a36b-95afd241906e
duration: 213
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '180'
ht-degree: 100%

---

# Adobe Target へのエクスペリエンスフラグメントの書き出し {#experience-fragment-target}

AEM エクスペリエンスフラグメントを Adobe Target オファーとして書き出す方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 次の手順

+ [エクスペリエンスフラグメントオファーを使用した Target アクティビティの作成](./create-target-activity.md)

## トラブルシューティング

### Target へのエクスペリエンスフラグメントの書き出しが失敗する

#### エラー

Adobe Admin Console で適切な権限を持たずにエクスペリエンスフラグメントを Adobe Target に書き出すと、AEM オーサーサービスで次のエラーが発生します。

![Target API UI エラー](assets/error-target-offer.png)

さらに、次のメッセージが `aemerror` ログに記録されます。

![Target API コンソールエラー](assets/target-console-error.png)

#### 解決策

1. AEM 統合で使用する Adobe Target 製品プロファイルの管理者権限で [Admin Console](https://adminconsole.adobe.com/) にログインします。
2. __製品／Adobe Target／製品プロファイル__&#x200B;を選択します
3. 「__統合__」タブで、AEM as a Cloud Service 環境用の統合（Adobe Developer プロジェクトと同じ名前）を選択します。
4. __編集者__&#x200B;または&#x200B;__承認者__&#x200B;の役割を割り当てます。

   ![Target API エラー](assets/target-permissions.png)

Adobe Target 統合に正しい権限を追加すると、このエラーが解決します。

## サポートリンク

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)
