---
title: 'AEM AssetsのInDesignファイルとアセットテンプレートについて '
description: このビデオチュートリアルでは、AEM Assetsのアセットテンプレート機能で使用するInDesignファイルの定義と、それに付随するすべての考慮事項について順を追って説明します。
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 1%

---


# AEM Assets{#understanding-indesign-files-and-asset-templates-in-aem-assets}のInDesignファイルとアセットテンプレートについて

このビデオチュートリアルでは、AEM Assetsのアセットテンプレート機能で使用するInDesignファイルの定義と、それに付随するすべての考慮事項について順を追って説明します。

## InDesignテンプレートファイルの構築{#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. [**InDesignファイルテンプレート**](assets/asset-templates-tutorial-video--supporting-files.zip)&#x200B;をダウンロードして開きます
2. **タグパネルを開き、タグの命名規則を** 確認します。InDesignファイル内の作成者可能な要素には既にタグが付いていることに注意してください。AEMでは、タグ付き要素のみを編集できます。

   * **ウィンドウ/ユーティリティ/タグ**

3. ページに、追加新しいテキスト要素を追加し、「Header」というテキストを指定して、**見出し**&#x200B;段落スタイルを適用します。

   * **ウィンドウ/スタイル/段落スタイル**

   次に、**Page2Heading.**&#x200B;という名前の新しいタグを作成し、適用します。

4. 追加マスターページのLogo要素にFPOロゴ画像（zip](assets/asset-templates-tutorial-video--supporting-files.zip)に含まれる[）を表示します。

   * **右クリックし**&#x200B;て、「継ぎ手」>「フレーム継ぎ手のオプション」。../コンテンツ継ぎ手」>「フレームの縦横の比率を維持して埋める」を&#x200B;**選択します**
   [フレームフィッティングオプションの詳細](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)（使用事例に最適）

5. 「ページ」のマスターテンプレートからヘッダー(「ロゴ」と「会社名」)をコピーし、「ページを同じ位置に貼り付け」を使用してページをコピーします。

   * 1ページ目で、Shift + Cmd + MacOSの場合はShift + Altキーを押しながらクリックし、マスターページから公開されるヘッダーを選択して削除します。
   * マスターページから、「同じ位置に貼り付け」を使用してヘッダーをページ1にコピーします
   * 2ページ目の手順を繰り返します

6. [構造]パネルを開き、各要素を重複クリックして、すべての構造要素がInDesignファイル内の実際の要素に対応していることを確認します。 使用されていない要素や不要な要素を削除します。 すべてのタグがセマンティックであり、要素が適切にタグ付けされていることを確認します。

   >[!NOTE]
   >
   >AEMアセットテンプレートでの問題の最も一般的な原因は、InDesignファイルの構造が不適切なことです。したがって、タグ付けと構造がクリーンで正しいことを確認してください。

## AEM Assets{#creating-and-authoring-an-asset-template-in-aem-assets}でのアセットテンプレートの作成とオーサリング

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **開始InDesign** サーバーのポートは8080です。
2. **AEM作成者インスタンスがInDesign Server**&#x200B;とやり取りするように設定されていることを確認します（逆も同じです）。

   * [IDSワーカーCloud Serviceの構成](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [クラウドプロキシCloud Serviceの設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGiの設定](http://localhost:4502/system/console/configMgr)

3. **InDesignファイルをAEM Assetにアップロードし、AEM** ワークフローとInDesign Serverがアセットを完全に処理できるようにします。
4. **アセット/テ** ンプレートの下に新しい **** InDesignを作成し、手順4でAEMにアップロードしたテンプレートファイルを選択します。
5. **手順5で作成したアセット** テンプレートを編集し、編集可能なフィールドを作成します。
6. 「**完了**」をクリックして、アセットテンプレートの最終的な高忠実度レンディションを生成します。
7. アセットテンプレートカードをクリックして開き、アセットレンディションを確認して高忠実度レンディションをダウンロードします。

## その他のリソース {#additional-resources}

InDesignテンプレートファイルとサポートする画像

[InDesignテンプレートファイルとサポートする画像](assets/asset-templates-tutorial-video--supporting-files-1.zip)をダウンロード

* [InDesignCC体験版のダウンロード](https://creative.adobe.com/products/download/indesign)
* [InDesign Server体験版のダウンロード](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
