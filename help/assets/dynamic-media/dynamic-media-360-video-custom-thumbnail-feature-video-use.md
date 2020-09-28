---
title: ダイナミックメディア360ビデオおよびカスタムビデオサムネールとAEM Assetsの使用
seo-title: ダイナミックメディア360ビデオおよびカスタムビデオサムネールとAEM Assetsの使用
description: AEM 6.5のダイナミックメディアビューアの機能強化には、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートおよびカスタムビデオサムネールの選択機能が追加されました。
seo-description: AEM 6.5のダイナミックメディアビューアの機能強化には、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートおよびカスタムビデオサムネールの選択機能が追加されました。
uuid: 44b91c22-635c-48c2-af27-49bdbfb61639
discoiquuid: 67d5e0f2-3fde-4ea7-9e53-4fc0cf8b9f2a
sub-product: dynamic-media
feature: video-profiles, viewer-presets
topics: images, videos, renditions, authoring, integrations, publishing, metadata
doc-type: feature video
audience: all
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 4%

---


# ダイナミックメディア360ビデオおよびカスタムビデオサムネールとAEM Assetsの使用

AEM 6.5のダイナミックメディアビューアの機能強化には、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートおよびカスタムビデオサムネールの選択機能が追加されました。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>ビデオでは、AEMインスタンスがダイナミックメディアS7モードで実行されていることを前提としています。  [ダイナミックメディアを使用したAEMの設定手順については、こちらを参照してください](https://helpx.adobe.com/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。 ビデオをアップロードする場合、デフォルトでは、フッテージの縦横比が2:1である場合、ダイナミックメディアは360ビデオとして処理します。 例えば、幅と高さの比率は2:1です。

>[!NOTE]
>
>ダイナミックメディア360メディアコンポーネントは、360ビデオのみをサポートします。

## ダイナミックメディア360ビデオ

360度ビデオ（球形ビデオとも呼ばれる）は、全方位カメラまたはカメラの集合を使って撮影し、各方向の表示を同時に記録するビデオ録画です。 フラットディスプレイでの再生中は、ユーザーが視聴方向を制御し、携帯端末での再生は通常、組み込みのジャイロスコープ制御を利用します。  1枚の写真の限界を超える範囲を広げることができます。 マーケターは、360本のビデオを使用して、ユーザーに魅力的なエクスペリエンスを提供できます。  始めましょう。 パノラマ画像の縦横比基準は、会社のDMS7設定で変更できます。変更するには、/conf/global/settings/cloudconfigs/dmscene7/jcr:contentで重複プロパティs7PanoramicARを指定します。

## ダイナミックメディア360ビデオ

ダイナミックメディアビデオで、ビデオのカスタムサムネールを選択できるようになりました。 ユーザは、AEM Assetsから既存のアセットを選択するか、ビデオフレームをサムネールとして選択できます。

## Dynamic 360メディアビューア

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**ビデオ360ソーシャルビューア**</td>
      <td>**ビデオ360VRビューア**</td>
   </tr>
   <tr>
      <td>ダイナミックメディア実行モード</td>
      <td>ダイナミックメディアScene7モードのみ</td>
      <td>ダイナミックメディアScene7モードのみ<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>使用例</td>
      <td>
         <p>ジャイロスコープをサポートしないWebサイトおよびデバイス用</p>
         <p> </p>
      </td>
      <td>
         <p>ジャイロスコープをサポートするデバイスに仮想現実体験を提供 </p>
      </td>
   </tr>
   <tr>
      <td>オーディオ — ステレオモード</td>
      <td>不可</td>
      <td>可</td>
   </tr>
   <tr>
      <td>ビデオ再生</td>
      <td>はい</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>表示地点ナビゲーション</td>
      <td>
         <ul>
            <li>マウスドラッグ（デスクトップシステムの場合）</li>
            <li>スワイプ（タッチデバイス）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>マウスとドラッグのオプションは無効です</li>
            <li>組み込みジャイロスコープを使用</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 Player</td>
      <td>はい</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>ソーシャルシェアのオプション</td>
      <td>はい</td>
      <td>不可</td>
   </tr>
</tbody>
</table>

## その他のリソース{#additional-resources}

[Scene7モードでのダイナミックメディアの設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)
