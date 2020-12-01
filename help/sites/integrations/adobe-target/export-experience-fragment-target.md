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


# エクスペリエンスフラグメントをAdobe Targetに書き出し{#experience-fragment-target}

AEMエクスペリエンスフラグメントをAdobe Targetオファーとして書き出す方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/41245?quality=12&learn=on)

## 次の手順

+ [エクスペリエンスフラグメントオファーを使用したターゲットアクティビティの作成](./create-target-activity.md)

## トラブルシューティング

### エクスペリエンスフラグメントのターゲットへの書き出しに失敗する

#### エラー

Adobe Admin Consoleで適切な権限を持たないエクスペリエンスフラグメントをAdobe Targetに書き出すと、AEM Authorサービスで次のエラーが発生します。

    ! [ターゲットAPI UIエラー](assets/error-target-offer.png)

...と`aemerror`ログの次のログメッセージ：

    ! [ターゲットAPIコンソールエラー](assets/target-console-error.png)

#### 解決方法

1. 使用しているAdobe Target製品プロファイルの管理権限で[Admin Console](https://adminconsole.adobe.com/)にログインしますが、AEM統合
2. __製品/Adobe Target/製品プロファイル__&#x200B;を選択します。
3. 「__統合__」タブで、AEMの統合をCloud Service環境として選択します(Adobe I/Oプロジェクトと同じ名前)。
4. __エディター__&#x200B;または&#x200B;__承認者__&#x200B;の役割を割り当てる

   ![ターゲットAPIエラー](assets/target-permissions.png)

このエラーは、Adobe Target統合に正しい権限を追加することで解決できます。

## サポートリンク

+ [Adobe Experience Cloudデバッガー — Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj)
+ [Adobe Experience Cloudデバッガ — Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/)