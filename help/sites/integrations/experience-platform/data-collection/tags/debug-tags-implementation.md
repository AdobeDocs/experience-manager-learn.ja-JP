---
title: タグ実装のデバッグ
description: タグ実装をデバッグするための一般的なツールと技術の紹介です。 ブラウザーのデベロッパーコンソールとExperience Platformデバッガー拡張機能を使用して、タグ実装の主要な側面を特定し、トラブルシューティングする方法について説明します。
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
kt: 6047
thumbnail: 38567.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
source-git-commit: 1d2daf53cd28fcd35cb2ea5c50e29b447790917a
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 5%

---

# タグ実装のデバッグ {#debug-tags-implementation}

タグ実装のデバッグに使用される一般的なツールとテクニックの概要です。 ブラウザーのデベロッパーコンソールとExperience Platformデバッガー拡張機能を使用して、タグ実装の主要な側面を特定し、トラブルシューティングする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Satellite オブジェクトを介したクライアント側デバッグ

クライアント側のデバッグは、タグプロパティのルールの読み込みまたは実行順序を確認するのに役立ちます。 Tag プロパティが Web サイトに追加されるたびに、 `_satellite` JavaScript オブジェクトがブラウザーに存在し、クライアント側のイベントとデータの追跡を容易にします。

クライアント側のデバッグを有効にするには、 `setDebug(true)` メソッド `_satellite` オブジェクト。

1. ブラウザーコンソールを開き、次のコマンドを実行します。

   ```javascript
       _satellite.setDebug(true);
   ```

1. AEMサイトページを再読み込みし、コンソールログに表示される内容を確認します。 _実行されたルール_ 以下のようなメッセージ。

   ![オーサーページとパブリッシュページのタグプロパティ](assets/satellite-object-debugging.png)

## Adobe Experience Platform Debugger を使用したデバッグ

AdobeがAdobe Experience Platform Debugger を提供する [Chrome 拡張機能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) および [Firefox アドオン](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) を使用して、統合をデバッグ、理解し、インサイトを得ることができます。

1. Adobe Experience Platform Debugger 拡張機能を開き、パブリッシュインスタンスでサイトページを開きます。

1. 内 **Adobe Experience Platform Debugger /概要/ Adobe Experience Platformタグ** 「 」セクションで、タグプロパティの詳細（名前、バージョン、ビルド日、環境、拡張機能など）を確認します。

   ![Adobe Experience Platform Debugger とタグプロパティの詳細](assets/tag-property-details.png)

## その他のリソース {#additional-resources}

+ [Adobe Experience Platform デバッガーの概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html)

+ [Satellite オブジェクトのリファレンス](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html)
