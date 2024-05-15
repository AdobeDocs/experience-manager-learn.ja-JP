---
title: AEM Assets の InDesign ファイルとアセットテンプレートについて
description: このビデオチュートリアルでは、AEM Assets のアセットテンプレート機能で使用する InDesign ファイルの定義と、それに伴うすべての考慮事項について順を追って説明します。
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 100%

---

# AEM Assets の InDesign ファイルとアセットテンプレートについて {#understanding-indesign-files-and-asset-templates-in-aem-assets}

このビデオチュートリアルでは、AEM Assets のアセットテンプレート機能で使用する InDesign ファイルの定義と、それに伴うすべての考慮事項について順を追って説明します。

## InDesign テンプレートファイルの作成 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. [**InDesign ファイルテンプレート**](assets/asset-templates-tutorial-video--supporting-files.zip)&#x200B;をダウンロードして開きます。
2. **タグパネルを開き**、タグの命名規則を確認します。InDesign ファイル内のオーサリング可能な要素には既にタグが付けられています。AEM では、タグ付きの要素のみを編集できます。

   * **ウィンドウ／ユーティリティ／タグ**

3. ページで新規テキスト要素を追加し、「Header」というテキストを指定して、「**見出し**」段落スタイルを適用します。

   * **ウィンドウ／スタイル／段落スタイル**

   次に、**Page2Heading.** という名前の新規タグを作成して適用します。

4. FPO ロゴ画像（[zip で提供](assets/asset-templates-tutorial-video--supporting-files.zip)）をマスターページのロゴ要素に追加します。

   * **右クリックし**、**調整／フレーム調整オプション...／コンテンツ調整／フレームに均等に流し込む**&#x200B;を選択します。

   フレーム調整オプションと現在のユースケースに適したオプションについて詳しくは、[こちら](https://helpx.adobe.com/jp/indesign/using/frames-objects.html#fitting_objects_to_frames)を参照してください。

5. ページ内のマスターテンプレートからヘッダー（ロゴと会社名）をコピーし、「同じ位置にペースト」を使用してページにペーストします。

   * 1 ページ目で、macOS の場合は Shift キーと Command キーを押しながらクリック、Windows の場合は Shift キーと Alt キーを押しながらクリックして、マスターページから公開されたヘッダーを選択して削除します。
   * マスターページから、「同じ位置にペースト」を使用してヘッダーを 1 ページ目にコピーします。
   * 2 ページ目に同じ手順を繰り返します。

6. 各要素をダブルクリックして構造パネルを開き、すべての構造要素が InDesign ファイル内の実際の要素に対応していることを確認します。使用されていない要素や不要な要素を削除します。すべてのタグ付けがセマンティックで、要素が適切にタグ付けされていることを確認します。

   >[!NOTE]
   >
   >AEM アセットテンプレートで問題が発生する最も一般的な原因は、InDesign ファイルの不適切な構造です。したがって、タグ付けと構造が明確で正しいことを確認します。

## AEM Assets でのアセットテンプレートの作成とオーサリング {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. ポート 8080 で **InDesign Server を起動**&#x200B;します。
2. **InDesign Server とやり取りするように AEM オーサーインスタンスが設定されている**&#x200B;ことを確認します（逆も同様）。

   * [IDS ワーカークラウドサービス設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [クラウドプロキシクラウドサービス設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi 設定](http://localhost:4502/system/console/configMgr)

3. **InDesign ファイルを AEM Assets にアップロードし**、AEM ワークフローと InDesign Server がアセットを完全に処理できるようにします。
4. **アセット／テンプレート**&#x200B;で&#x200B;**新規テンプレートを作成**&#x200B;し、手順 4 で AEM にアップロードした InDesign ファイルを選択します。
5. 手順 5 で作成した&#x200B;**アセットテンプレートを編集**&#x200B;し、編集可能フィールドをオーサリングします。
6. 「**完了**」をクリックして、アセットテンプレートの忠実度の高い最終的なレンディションを生成します。
7. アセットテンプレートカードをクリックして開き、アセットレンディションを確認して、忠実度の高いレンディションをダウンロードします。

## その他のリソース {#additional-resources}

InDesign のテンプレートファイルとサポート画像

[InDesign のテンプレートファイルとサポート画像](assets/asset-templates-tutorial-video--supporting-files-1.zip)をダウンロードします。

* [InDesign CC 体験版のダウンロード](https://creative.adobe.com/products/download/indesign)
* InDesign Server 体験版は、[アドビプレリリースサイト](https://www.adobeprerelease.com/)からダウンロードできます。または、[CC エンタープライズ版のお客様は、アカウント担当者に連絡して、InDesign Server 体験版ライセンスをリクエストできます](https://www.adobe.com/jp/products/indesignserver/faq.html)。
