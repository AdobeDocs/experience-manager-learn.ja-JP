---
title: AEM Assets での Dynamic Media 360 ビデオとカスタムビデオサムネールの使用
description: AEM 6.5 の Dynamic Media ビューアの機能強化には、360 ビデオレンダリング、360 メディアビューア（video360Social および video360VR）のサポートと、カスタムビデオサムネールを選択する機能が含まれています。
feature: Video Profiles
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 4ee0b68f-3897-4104-8615-9de8dbb8f327
duration: 656
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '394'
ht-degree: 100%

---

# AEM Assets での Dynamic Media 360 ビデオとカスタムビデオサムネールの使用

AEM 6.5 の Dynamic Media ビューアの機能強化には、360 ビデオレンダリング、360 メディアビューア（video360Social および video360VR）のサポートと、カスタムビデオサムネールを選択する機能が含まれています。

>[!VIDEO](https://video.tv.adobe.com/v/34575?quality=12&learn=on&captions=jpn)

>[!NOTE]
>
>ビデオでは、AEM インスタンスが Dynamic Media S7 モードで動作していることを前提としています。  [AEM と Dynamic Media の連携のセットアップ手順については、こちらを参照してください](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。ビデオをアップロードすると、デフォルトでは、Dynamic Media はフッテージの縦横比が 2:1 の場合、360 ビデオとして処理します。 つまり、幅と高さの比率は 2:1 です。

>[!NOTE]
>
>Dynamic Media 360 メディアコンポーネントでは、360 ビデオのみをサポートしています。

## Dynamic Media 360 ビデオ

360 ビデオ（球形ビデオとも呼ばれる）は、全方向のビューが同時に記録されるビデオ録画で、全方向のカメラやカメラのコレクションを使用して撮影されます。フラットディスプレイでの再生時には、ユーザーは視野角を制御できます。また、モバイルデバイスでの再生では通常、デバイス組み込みのジャイロスコープ制御を活用します。1 枚の写真の限界を超えて広げることができます。 マーケターは、360 ビデオを利用して、ユーザーを魅力的なエクスペリエンスに導くことができます。それでは、始めましょう。パノラマ画像の縦横比の基準は、会社の DMS7 設定で、 /conf/global/settings/cloudconfigs/dmscene7/jcr:content にある double プロパティ s7PanoramicAR を指定することで変更できます。

## Dynamic Media 360 ビデオ

Dynamic Media ビデオで、ビデオのカスタムサムネールを選択する機能がサポートされるようになりました。 ユーザーは、AEM Assets から既存のアセットを選択するか、サムネールとしてビデオフレームを選択できます。

## Dynamic 360 メディアビューア

<table> 
 <tbody>
   <tr>
      <td>  </td>
      <td>**Video360Social ビューア**</td>
      <td>**Video360VR ビューア**</td>
   </tr>
   <tr>
      <td>Dynamic Media 実行モード</td>
      <td>Dynamic Media Scene7 モードのみ</td>
      <td>Dynamic Media Scene7 モードのみ<br>
 <br>
      </td>
   </tr>
   <tr>
      <td>ユースケース</td>
      <td>
         <p>ジャイロスコープをサポートしていない web サイトやデバイスの場合</p>
         <p> </p>
      </td>
      <td>
         <p>ジャイロスコープをサポートしているデバイスに仮想現実エクスペリエンスを提供 </p>
      </td>
   </tr>
   <tr>
      <td>オーディオ - ステレオモード</td>
      <td>いいえ</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>ビデオの再生</td>
      <td>はい</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>POV ナビゲーション</td>
      <td>
         <ul>
            <li>マウスドラッグ（デスクトップシステム上）</li>
            <li>スワイプ（タッチデバイス）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>マウスとドラッグのオプションは無効です</li>
            <li>組み込みのジャイロスコープを使用</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5 プレーヤー</td>
      <td>はい</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>ソーシャル共有オプション</td>
      <td>はい</td>
      <td>いいえ</td>
   </tr>
</tbody>
</table>

## その他のリソース{#additional-resources}

[Scene7 モードでの Dynamic Media の設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)
