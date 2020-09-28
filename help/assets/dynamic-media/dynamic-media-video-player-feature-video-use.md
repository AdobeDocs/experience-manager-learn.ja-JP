---
title: AEM Dynamic Media でのビデオプレーヤーの使用
seo-title: AEM Dynamic Media でのビデオプレーヤーの使用
description: デスクトップクライアントやブラウザでのアダプティブビデオストリーミングをサポートするために、Flashランタイムに依存していたAEM Dynamic Mediaビデオプレーヤーは、Flashベースのコンテンツストリーミングに対して積極的になりました。 HLS(AppleのHTTP Live Streamingビデオ配信プロトコル)の導入により、フラッシュに依存せずにコンテンツをストリーミングできるようになりました。
seo-description: デスクトップクライアントやブラウザでのアダプティブビデオストリーミングをサポートするために、Flashランタイムに依存していたAEM Dynamic Mediaビデオプレーヤーは、Flashベースのコンテンツストリーミングに対して積極的になりました。 HLS(AppleのHTTP Live Streamingビデオ配信プロトコル)の導入により、フラッシュに依存せずにコンテンツをストリーミングできるようになりました。
uuid: aac6f471-4bed-4773-890f-0dd2ceee381d
discoiquuid: b01cc46b-ef64-4db9-b3b4-52d3f27bddf5
sub-product: dynamic-media
feature: media-player, video-profiles
topics: videos, renditions, authoring, best-practices
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 26%

---


# AEM Dynamic Media でのビデオプレーヤーの使用{#using-the-video-player-in-aem-dynamic-media}

デスクトップクライアントやブラウザでのアダプティブビデオストリーミングをサポートするために、Flashランタイムに依存していたAEM Dynamic Mediaビデオプレーヤーは、Flashベースのコンテンツストリーミングに対して積極的になりました。 HLS(AppleのHTTP Live Streamingビデオ配信プロトコル)の導入により、フラッシュに依存せずにコンテンツをストリーミングできるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## Flash以外のビデオプレーヤーの概要 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLSブラウザーのサポートは次のとおりです。サポートされていないブラウザーの場合、プログレッシブビデオ配信にフォールバックします。

<table> 
 <thead> 
  <tr> 
   <th> <p>デバイス</p> </th>
   <th> <p>ブラウザー</p> </th>
   <th > <p>ビデオ再生モード</p> </th>
  </tr>
 </thead>
 <tbody>
  <tr> 
   <td> <p>デスクトップ</p> </td>
   <td> <p>Internet Explorer 9 および 10</p> </td>
   <td> <p>プログレッシブダウンロード</p> </td>
  </tr>
  <tr>
   <td> <p>デスクトップ</p> </td>
   <td> <p>Internet Explorer 11+</p> </td>
   <td> <p>HLSビデオストリーミング</p> </td>
  </tr>
  <tr>
   <td> <p>デスクトップ</p> </td>
   <td> <p>Firefox 23～44</p> </td>
   <td> <p>プログレッシブダウンロード</p> </td>
  </tr>
  <tr> 
   <td> <p>デスクトップ</p> </td>
   <td> <p>Firefox 45 以降</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>デスクトップ</p> </td>
   <td> <p>Chrome</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>デスクトップ</p> </td>
   <td> <p>Safari（Mac）</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Chrome（Android 6 以前）</p> </td>
   <td> <p>プログレッシブダウンロード</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Chrome（Android 7 以降）</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Android（デフォルトのブラウザー）</p> </td>
   <td> <p>プログレッシブダウンロード</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Safari（iOS）</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Chrome（iOS）</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Blackberry</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
 </tbody>
</table>