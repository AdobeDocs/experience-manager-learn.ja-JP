---
title: エクスペリエンスフラグメントのAdobe Targetへの書き出し
description: AEMエクスペリエンスフラグメントをAdobe Targetオファーとして発行および書き出す方法について説明します。
feature: experience-fragments
topics: integrations, authoring
audience: all
doc-type: feature video
activity: use
version: cloud-service
kt: 6350
thumbnail: 41245.jpg
translation-type: tm+mt
source-git-commit: 988e390dd9e1fc6033b3651db151e6a60ce4efaa
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 3%

---


# Export Experience Fragment to Adobe Target {#experience-fragment-target}

AEMエクスペリエンスフラグメントをAdobe Targetオファーとして書き出す方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 次の手順

+ [エクスペリエンスフラグメントオファーを使用したターゲットアクティビティの作成](./create-target-activity.md)

## トラブルシューティング

### エクスペリエンスフラグメントのターゲットへの書き出しに失敗する

#### エラー

Adobe Admin Consoleで適切な権限を持たないエクスペリエンスフラグメントをAdobe Targetに書き出すと、AEM Authorサービスで次のエラーが発生します。

    ! [ターゲットAPI UIエラー](assets/error-target-offer.png)

...および次のログメッセージが `aemerror` ログに記録されます。

    ! [ターゲットAPIコンソールエラー](assets/target-console-error.png)

#### 解決方法

1. 使用しているが、AEM統合では、 [Admin Consoleに対して管理権限を持つAdobe Target製品プロファイルにログインする](https://adminconsole.adobe.com/)
2. 「 __商品」>「Adobe Target」>「製品プロファイル」を選択します。__
3. 「 __Integrations__ 」タブで、AEM用のCloud Service環境(AdobeI/Oプロジェクトと同じ名前)を選択します。
4. エ __ディタ__ または __承認者の役割の割り当て__

   ![ターゲットAPIエラー](assets/target-permissions.png)

このエラーは、Adobe Target統合に正しい権限を追加することで解決できます。

## サポートリンク

+ [Adobe Experience Cloudデバッガー — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloudデバッガ — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)