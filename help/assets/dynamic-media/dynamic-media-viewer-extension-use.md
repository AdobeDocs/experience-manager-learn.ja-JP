---
title: Adobe AnalyticsとAdobeの起動でのDynamic Mediaビューアの使用
description: Dynamic Media ビューア 5.13 のリリースと共に、Adobe Launch の Dynamic Media ビューア拡張機能を使用すると、Dynamic Media、Adobe Analytics、Adobe Launch のユーザーは、Adobe Launch 設定で Dynamic Media ビューア固有のイベントとデータを使用できます。
sub-product: Dynamic Media
feature: Asset Insights
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 28%

---


# Adobe AnalyticsとAdobeでのDynamic Mediaビューアの使用{#using-dynamic-media-viewers-adobe-analytics-launch}

Dynamic MediaとAdobe Analyticsをご利用のお客様は、Dynamic Mediaビューア拡張機能を使用して、Webサイト上のDynamic Mediaビューアの使用状況を追跡できるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/29308/?quality=12&learn=on)

>[!NOTE]
>
> この機能には、Dynamic MediaScene7モードでAdobe Experience Managerを実行してください。 また、[Adobe Experience Platform LaunchをAEMインスタンス](https://docs.adobe.com/content/help/ja-JP/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)と統合する必要もあります。

Dynamic Mediaビューア拡張機能の導入に伴い、Adobe Experience ManagerオファーはDynamic Mediaビューアで提供されるアセットの高度な解析サポート(5.13)を行い、Dynamic Mediaビューアをサイトページで使用する際のイベント追跡をより詳細に制御できるようになりました。

既にAEM Assetsとサイトをお持ちの場合は、起動プロパティをAEM作成者インスタンスと統合できます。 起動の統合がWebサイトに関連付けられると、ビューアのイベント追跡を有効にして、動的メディアコンポーネントをページに追加できます。

AEM Assetsのみのお客様またはDynamic Mediaクラシックのお客様は、ビューアの埋め込みコードを取得して、ページに追加できます。 その後、起動スクリプトライブラリをページに手動で追加して、ビューアのイベントを追跡できます。

次の表に、Dynamic Media ビューアイベントと、サポートされている引数を示します。

<table>
   <tbody>
      <tr>
         <td>ビューアのイベント名</td>
         <td>引数の参照</td>
      </tr>
      <tr>
         <td> 共通 </td>
         <td> %event.detail.dm.objID% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.compClass% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.instName% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.timeStamp% </td>
      </tr>
      <tr>
         <td> バナー <br></td>
         <td> %event.detail.dm.BANNER.asset% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.BANNER.label% </td>
      </tr>
      <tr>
         <td> HREF </td>
         <td> %event.detail.dm.HREF.rollover% </td>
      </tr>
      <tr>
         <td> 項目 </td>
         <td> %event.detail.dm.ITEM.rollover% </td>
      </tr>
      <tr>
         <td> LOAD </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.会社% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.sdkversion% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewertype% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.viewerversion% </td>
      </tr>
      <tr>
         <td> METADATA </td>
         <td> %event.detail.dm.METADATA.length% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.METADATA.type% </td>
      </tr>
      <tr>
         <td> MILESTONE </td>
         <td> %event.detail.dm.MILESTONE.milestone% </td>
      </tr>
      <tr>
         <td> ページ </td>
         <td> %event.detail.dm.PAGE.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.PAGE.label% </td>
      </tr>
      <tr>
         <td> PAUSE </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> PLAY </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> SPIN </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> STOP </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> SWAP </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> SWATCH </td>
         <td> %event.detail.dm.SWATCH.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.SWATCH.label% </td>
      </tr>
      <tr>
         <td> TARG </td>
         <td> %event.detail.dm.TARG.frame% </td>
      </tr>
      <tr>
         <td> </td>
         <td> %event.detail.dm.TARG.label% </td>
      </tr>
      <tr>
         <td> ズーム </td>
         <td> %event.detail.dm.ZOOM.scale% </td>
      </tr>
   </tbody>
</table>

## その他のリソース{#additional-resources}

* [Adobe Experience ManagerとAdobe発表の統合](https://docs.adobe.com/content/help/en/experience-manager-learn/sites/integrations/adobe-launch-integration-tutorial-understand.html)
* [Dynamic MediaScene7モードでAdobe Experience Managerを走る](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)
* [Dynamic Media ビューアと Adobe Analytics および Adobe Launch の統合](https://helpx.adobe.com/experience-manager/6-5/assets/using/launch.html)
