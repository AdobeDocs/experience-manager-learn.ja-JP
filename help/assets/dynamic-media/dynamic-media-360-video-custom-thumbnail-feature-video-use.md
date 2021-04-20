---
title: AEM AssetsでのDynamic Media360ビデオとカスタムビデオサムネールの使用
description: AEM 6.5でのDynamic Mediaビューアの機能強化に加え、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートおよびカスタムビデオサムネールの選択機能が追加されました。
sub-product: dynamic-media
feature: ビデオプロファイル
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 7%

---


# AEM AssetsでのDynamic Media360ビデオとカスタムビデオサムネールの使用

AEM 6.5でのDynamic Mediaビューアの機能強化に加え、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートおよびカスタムビデオサムネールの選択機能が追加されました。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>ビデオでは、AEMインスタンスがDynamic MediaS7モードで実行されていることを前提としています。  [Dynamic MediaでAEMを設定する手順は、こちらを参照してください](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。ビデオをアップロードする場合、縦横比が2:1の場合、Dynamic Mediaは初期設定で360ビデオとしてフッテージを処理します。 例えば、幅と高さの比率は2:1です。

>[!NOTE]
>
>Dynamic Media360メディアコンポーネントは360ビデオのみをサポートしています。

## Dynamic Media360ビデオ

360度ビデオ（球形ビデオとも呼ばれる）は、全方位カメラまたはカメラの集合を使って撮影し、各方向の表示を同時に記録するビデオ録画です。 フラットディスプレイでの再生中は、ユーザーが視聴方向を制御し、携帯端末での再生は通常、組み込みのジャイロスコープ制御を利用します。  1枚の写真の限界を超える範囲を広げることができます。 マーケターは、360本のビデオを使用して、ユーザーに魅力的なエクスペリエンスを提供できます。  始めましょう。 パノラマ画像の縦横比基準は、会社のDMS7設定で変更できます。変更するには、/conf/global/settings/cloudconfigs/dmscene7/jcr:contentで重複プロパティs7PanoramicARを指定します。

## Dynamic Media360ビデオ

Dynamic Mediaビデオで、ビデオのカスタムサムネールを選択できるようになりました。 ユーザは、AEM Assetsから既存のアセットを選択するか、ビデオフレームをサムネールとして選択できます。

## Dynamic 360メディアビューア

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**ビデオ360ソーシャルビューア**</td>
      <td>**ビデオ360VRビューア**</td>
   </tr>
   <tr>
      <td>Dynamic Media実行モード</td>
      <td>Dynamic MediaScene7モードのみ</td>
      <td>Dynamic MediaScene7モードのみ<br>
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
      <td>いいえ</td>
   </tr>
</tbody>
</table>

## その他のリソース{#additional-resources}

[Scene7モードでのDynamic Mediaの設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)
