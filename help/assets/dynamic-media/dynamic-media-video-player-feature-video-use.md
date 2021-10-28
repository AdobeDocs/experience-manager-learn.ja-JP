---
title: AEM Dynamic Media でのビデオプレーヤーの使用
description: AEM Dynamic Media video player は、デスクトップクライアントやブラウザーでのアダプティブビデオストリーミングをサポートするためにFlashランタイムに依存していましたが、flash ベースのコンテンツストリーミングに対して積極的に取り組むようになりました。 HLS(Appleの HTTP ライブストリーミングビデオ配信プロトコル ) の導入に伴い、Flash に依存せずにコンテンツをストリーミングできるようになりました。
sub-product: dynamic-media
feature: Video Profiles
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
source-git-commit: d8738ef33940efc856e6fc00f6a57abb641598e5
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 28%

---

# AEM Dynamic Media でのビデオプレーヤーの使用{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media video player は、デスクトップクライアントやブラウザーでのアダプティブビデオストリーミングをサポートするためにFlashランタイムに依存していましたが、flash ベースのコンテンツストリーミングに対して積極的に取り組むようになりました。 HLS(Appleの HTTP ライブストリーミングビデオ配信プロトコル ) の導入に伴い、Flash に依存せずにコンテンツをストリーミングできるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16791/?quality=9&learn=on)

## 非Flashビデオプレーヤーのクイック参照 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429/?quality=9&learn=on)

HLS ブラウザーのサポートは次のとおりです。サポートされていないブラウザーの場合、アドビはプログレッシブビデオ配信にフォールバックします。

>[!NOTE]
>
> Dynamic Media Hybrid は、2022 年 5 月以降、Internet Explorer 11 をサポートしません。

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
   <td> <p>HLS ビデオストリーミング</p> </td>
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
