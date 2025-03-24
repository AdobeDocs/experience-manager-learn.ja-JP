---
title: Dynamic Media Viewer と Adobe Analytics およびタグの使用
description: Dynamic Media Viewer 5.13 のリリースと共に、タグの Dynamic Media Viewer 拡張機能を使用すると、Dynamic Media、Adobe Analytics、タグのユーザーは、タグ設定で Dynamic Media Viewer 固有のイベントとデータを使用できます。
sub-product: Dynamic Media
feature: Asset Insights
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 9d807f4c-999c-45e6-a9db-6c1776bddda1
duration: 576
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 100%

---

# Dynamic Media Viewer と Adobe Analytics およびタグの使用{#using-dynamic-media-viewers-adobe-analytics-tags}

Dynamic Media と Adobe Analytics をご利用のお客様は、Dynamic Media Viewer 拡張機能を使用して、web サイト上での Dynamic Media Viewer の使用状況を追跡できるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/29308?quality=12&learn=on)

>[!NOTE]
>
> この機能を利用するには、Dynamic Media Scene7 モードで Adobe Experience Manager を実行します。また、[タグと AEM インスタンスを統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja)する必要があります。

Dynamic Media Viewer 拡張機能の導入に伴い、Adobe Experience Manager では、Dynamic Media Viewer (5.13) で配信されるアセットの高度な分析サポートを提供し、Dynamic Media Viewer を Sites ページで使用した場合のイベント追跡をより詳細に制御できるようになりました。

既に AEM Assets と Sites を持っている場合は、タグプロパティを AEM オーサーインスタンスと統合できます。Experience Platform Launch の統合を web サイトに関連付けたら、ビューアのイベント追跡を有効にして、Dynamic Media コンポーネントをページに追加できます。

AEM Assets のみのお客様または Dynamic Media Classic のお客様は、ビューアの埋め込みコードを取得して、ページに追加できます。その後、タグスクリプトライブラリを手動でページに追加することで、ビューアイベントを追跡できるようになります。

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
         <td> マイルストーン </td>
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

* [AEM と Adobe Experience Platform のタグの統合](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/integrations/experience-platform-launch/overview.html?lang=ja)
* [Dynamic Media Scene7 モードでの Adobe Experience Manager の実行](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dms7.html?lang=ja)
* [タグを使用した Dynamic Media Viewe と Adobe Analytics の統合](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/dynamic-media/dynamic-media-viewer-extension-use.html?lang=ja)
