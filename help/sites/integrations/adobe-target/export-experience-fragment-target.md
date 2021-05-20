---
title: エクスペリエンスフラグメントのAdobe Targetへの書き出し
description: AEMエクスペリエンスフラグメントをAdobe Targetオファーとして公開および書き出す方法について説明します。
feature: エクスペリエンスフラグメント
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
topic: 統合
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 5%

---


# エクスペリエンスフラグメントをAdobe Targetに書き出す{#experience-fragment-target}

AEMエクスペリエンスフラグメントをAdobe Targetオファーとして書き出す方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 次の手順

+ [エクスペリエンスフラグメントオファーを使用したTargetアクティビティの作成](./create-target-activity.md)

## トラブルシューティング

### エクスペリエンスフラグメントをTargetに書き出せない

#### エラー

Adobe Admin Consoleで適切な権限を指定せずにAdobe Targetにエクスペリエンスフラグメントを書き出すと、AEMオーサーサービスで次のエラーが発生します。

    ! [Target API UIエラー](assets/error-target-offer.png)

と`aemerror`ログに次のログメッセージが記録されます。

    ! [Target APIコンソールエラー](assets/target-console-error.png)

#### 解決方法

1. AEM統合で使用しているAdobe Target製品プロファイルの管理権限で[Admin Console](https://adminconsole.adobe.com/)にログインします。
2. __Products/Adobe Target/Product Profile__&#x200B;を選択します。
3. 「__統合__」タブで、AEM as aCloud Service環境用の統合を選択します(Adobe I/Oプロジェクトと同じ名前)。
4. __編集者__&#x200B;または&#x200B;__承認者__&#x200B;の役割を割り当てる

   ![Target APIエラー](assets/target-permissions.png)

このエラーは、Adobe Target統合に正しい権限を追加することで解決できます。

## サポートリンク

+ [Adobe Experience Cloud Debugger - Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloud Debugger - Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)