---
title: タグ実装のデバッグ
description: タグ実装のデバッグに使用されるいくつかの一般的なツールと技術を紹介します。ブラウザーの Developer Console と Experience Platform デバッガー拡張機能を使用して、タグ実装の重要な側面を特定してトラブルシューティングする方法について説明します。
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-6047
thumbnail: 38567.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 647447ca-3c29-4efe-bb3a-d3f53a936a2a
duration: 272
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 100%

---

# タグ実装のデバッグ {#debug-tags-implementation}

タグ実装のデバッグに使用される一般的なツールと技術について紹介します。ブラウザーの Developer Console と Experience Platform デバッガー拡張機能を使用して、タグ実装の重要な側面を特定してトラブルシューティングする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/38567?quality=12&learn=on)

## Satellite オブジェクトを介したクライアントサイドのデバッグ

クライアントサイドのデバッグは、タグプロパティのルールの読み込みまたは実行順序を確認するのに役立ちます。タグプロパティが web サイトに追加されるたびに、`_satellite` JavaScript オブジェクトがブラウザーに存在するようになり、クライアントサイドのイベントとデータの追跡を容易にします。

クライアントサイドのデバッグを有効にするには、`_satellite` オブジェクトの `setDebug(true)` メソッドを呼び出します。

1. ブラウザーコンソールを開き、次のコマンドを実行します。

   ```javascript
       _satellite.setDebug(true);
   ```

1. AEM サイトページをリロードし、コンソールログに以下のような&#x200B;_ルール発動_&#x200B;メッセージが表示されていることを確認します。

   ![オーサーページとパブリッシュページのタグプロパティ](assets/satellite-object-debugging.png)

## Adobe Experience Platform Debugger を使用したデバッグ

アドビでは、統合をデバッグし、理解し、インサイトを得ることができるように、Adobe Experience Platform Debugger の [Chrome 拡張機能](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)と [Firefox アドオン](https://addons.mozilla.org/en-US/firefox/addon/adobe-experience-platform-dbg/) を提供しています。

1. Adobe Experience Platform Debugger の拡張機能を開き、パブリッシュインスタンスでサイトページを開きます。

1. **Adobe Experience Platform Debugger／サマリ／Adobe Experience Platform タグ**&#x200B;セクションで、名前、バージョン、ビルド日、環境、拡張機能などのタグプロパティの詳細を確認します。

   ![Adobe Experience Platform Debugger とタグプロパティの詳細](assets/tag-property-details.png)

## その他のリソース {#additional-resources}

+ [Adobe Experience Platform デバッガーの概要](https://experienceleague.adobe.com/docs/platform-learn/data-collection/debugger/overview.html?lang=ja)

+ [Satellite オブジェクトのリファレンス](https://experienceleague.adobe.com/docs/experience-platform/tags/client-side/satellite-object.html?lang=ja)
