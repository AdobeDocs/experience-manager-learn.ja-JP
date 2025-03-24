---
title: AEM Dynamic Media でのビデオプレーヤーの使用
description: AEM Dynamic Media ビデオプレーヤーは、デスクトップクライアントやブラウザーでのアダプティブビデオストリーミングをサポートするために Flash ランタイムに依存していましたが、Flash ベースのコンテンツストリーミングを積極的に導入するようになりました。HLS（Apple の HTTP ライブストリーミングビデオ配信プロトコル）の導入に伴い、Flash に依存せずにコンテンツをストリーミングできるようになりました。
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 7e4cb782-836d-4ec0-97d0-645b91ea43e0
duration: 568
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 100%

---


# AEM Dynamic Media でのビデオプレーヤーの使用{#using-the-video-player-in-aem-dynamic-media}

AEM Dynamic Media ビデオプレーヤーは、デスクトップクライアントやブラウザーでのアダプティブビデオストリーミングをサポートするために Flash ランタイムに依存していましたが、Flash ベースのコンテンツストリーミングを積極的に導入するようになりました。HLS（Apple の HTTP ライブストリーミングビデオ配信プロトコル）の導入に伴い、Flash に依存せずにコンテンツをストリーミングできるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16791?quality=12&learn=on)

## Flash に依存しないビデオプレーヤーの概要 {#quick-look-into-non-flash-video-player}

>[!VIDEO](https://video.tv.adobe.com/v/17429?quality=12&learn=on)

HLS ブラウザーのサポートは次のとおりです。サポートされていないブラウザーについては、プログレッシブビデオ配信にフォールバックします。

>[!NOTE]
>
> Dynamic Media Hybrid では、2022年3月15日（PT）現在、Internet Explorer 11 でのビデオストリーミングをサポートしていません。IE 11 でプログレッシブ再生にフォールバックするには、6.5.12 以上にアップグレードしてください。

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
   <td> <p>Dynamic Media - Scene7 モード：HLS ビデオストリーミング</p> 
        <p>Dynamic Media - ハイブリッドモード：プログレッシブダウンロード</p>
   </td>
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
   <td> <p>Chrome（Android™ 6 以前）</p> </td>
   <td> <p>プログレッシブダウンロード</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Chrome（Android™ 7 以降）</p> </td>
   <td> <p>HLS ビデオストリーミング</p> </td>
  </tr>
  <tr> 
   <td> <p>モバイル</p> </td>
   <td> <p>Android（デフォルトブラウザー）</p> </td>
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
