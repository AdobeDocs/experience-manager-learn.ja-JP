---
title: AEM Dynamic Media でのビデオプレーヤーの使用
description: AEM Dynamic Media video playerは、デスクトップクライアントやブラウザーでのアダプティブビデオストリーミングをサポートするためにFlashランタイムを利用していましたが、flashベースのコンテンツストリーミングに対して積極的に取り組むようになりました。 HLS（AppleのHTTPライブストリーミングビデオ配信プロトコル）の導入に伴い、フラッシュに依存せずにコンテンツをストリーミングできるようになりました。
sub-product: dynamic-media
feature: ビデオプロファイル
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '218'
ht-degree: 31%

---


# AEM Dynamic Media でのビデオプレーヤーの使用{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media video playerは、デスクトップクライアントやブラウザーでのアダプティブビデオストリーミングをサポートするためにFlashランタイムを利用していましたが、flashベースのコンテンツストリーミングに対して積極的に取り組むようになりました。 HLS（AppleのHTTPライブストリーミングビデオ配信プロトコル）の導入に伴い、フラッシュに依存せずにコンテンツをストリーミングできるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 非Flashビデオプレーヤー{#quick-look-into-non-flash-video-player}を簡単に確認します。

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLSブラウザーのサポートは次のとおりです。サポートされていないブラウザーの場合、アドビはプログレッシブビデオ配信にフォールバックします。

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
   <td> <p>HLSビデオストリーミング</p> </td>
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