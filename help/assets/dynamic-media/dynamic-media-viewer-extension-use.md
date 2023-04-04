---
title: Adobe AnalyticsとAdobeLaunch でのDynamic Media Viewers の使用
description: Dynamic Media ビューア 5.13 のリリースと共に、Adobe Launch の Dynamic Media ビューア拡張機能を使用すると、Dynamic Media、Adobe Analytics、Adobe Launch のユーザーは、Adobe Launch 設定で Dynamic Media ビューア固有のイベントとデータを使用できます。
sub-product: Dynamic Media
feature: Asset Insights
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 27%

---

# Adobe AnalyticsとAdobeLaunch でのDynamic Media Viewers の使用{#using-dynamic-media-viewers-adobe-analytics-launch}

Dynamic MediaとAdobe Analyticsを使用しているお客様は、Dynamic Mediaビューア拡張機能を使用して、Web サイト上でのDynamic Mediaビューアの使用状況を追跡できるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> この機能では、Dynamic Media Scene7モードでAdobe Experience Managerを実行します。 また、 [Adobe Experience Platform LaunchとAEMインスタンスの統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja).

Dynamic Media Viewer 拡張機能の導入に伴い、Adobe Experience Managerでは、Dynamic Mediaビューア (5.13) で配信されるアセットの高度な分析サポートを提供し、Dynamic Mediaビューアを Sites ページで使用した場合のイベント追跡をより詳細に制御できるようになりました。

既にAEM Assetsと Sites がある場合は、Launch プロパティをAEMオーサーインスタンスと統合できます。 Launch の統合を Web サイトに関連付けたら、ビューアのイベント追跡を有効にして、Dynamic Media コンポーネントをページに追加できます。

AEM Assetsのみのお客様またはDynamic Media Classicのお客様は、ビューアの埋め込みコードを取得して、ページに追加できます。 その後、ビューアイベントを追跡するために、Launch スクリプトライブラリを手動でページに追加できます。

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
         <td> 読み込み </td>
         <td> %event.detail.dm.LOAD.applicationname% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.asset% </td>
      </tr>
      <tr>
         <td><strong> </strong></td>
         <td> %event.detail.dm.LOAD.company% </td>
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
         <td> メタデータ </td>
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
         <td> 一時停止 </td>
         <td> %event.detail.dm.PAUSE.timestamp% </td>
      </tr>
      <tr>
         <td> 再生 </td>
         <td> %event.detail.dm.PLAY.timestamp% </td>
      </tr>
      <tr>
         <td> スピン </td>
         <td> %event.detail.dm.SPIN.framenumber% </td>
      </tr>
      <tr>
         <td> 停止 </td>
         <td> %event.detail.dm.STOP.timestamp% </td>
      </tr>
      <tr>
         <td> スワップ </td>
         <td> %event.detail.dm.SWAP.asset% </td>
      </tr>
      <tr>
         <td> スウォッチ </td>
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

* [Adobe Experience ManagerとAdobeLaunch の統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja)
* [Dynamic Media Scene7モードでのAdobe Experience Managerの実行](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=ja)
* [Dynamic Media ビューアと Adobe Analytics および Adobe Launch の統合](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html)
