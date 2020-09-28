---
title: 'AEM AssetsのInDesignファイルとアセットテンプレートについて '
description: このビデオチュートリアルでは、AEM Assetsのアセットテンプレート機能で使用するInDesignファイルの定義と、それに付随するすべての考慮事項について順を追って説明します。
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 1%

---


# AEM AssetsのInDesignファイルとアセットテンプレートについて {#understanding-indesign-files-and-asset-templates-in-aem-assets}

このビデオチュートリアルでは、AEM Assetsのアセットテンプレート機能で使用するInDesignファイルの定義と、それに付随するすべての考慮事項について順を追って説明します。

## InDesignテンプレートファイルの作成 {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. [**InDesignファイルテンプレートをダウンロードして開く**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **タグパネルを開き、タグの命名規則を確認し** 、InDesignファイル内の作成者可能な要素には既にタグが付いていることに注意してください。 AEMでは、タグ付き要素のみを編集できます。

   * **ウィンドウ/ユーティリティ/タグ**

3. 「ページ」追加に、新しいテキスト要素「ヘッダー」を入力し、「 **見出し** 」段落スタイルを適用します。

   * **ウィンドウ/スタイル/段落スタイル**

   次に、 **Page2Headingという名前の新しいタグを作成して適用します。**

4. FPO追加ロゴ画像(zipで[](assets/asset-templates-tutorial-video--supporting-files.zip)提供)を、マスターページのLogo要素に追加します。

   * **右クリック**&#x200B;し、「継ぎ手」**/「フレーム継ぎ手のオプション」。../コンテンツ継ぎ手/フレームの縦横の比率を維持」を選択します。**
   [フレームフィッティングオプションの詳細](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)（使用事例に最適）

5. 「ページ」のマスターテンプレートからヘッダー(「ロゴ」と「会社名」)をコピーし、「ページを同じ位置に貼り付け」を使用してページをコピーします。

   * 1ページ目で、Shift + Cmd + MacOSの場合はShift + Altキーを押しながらクリックし、マスターページから公開されるヘッダーを選択して削除します。
   * マスターページから、「同じ位置に貼り付け」を使用してヘッダーをページ1にコピーします
   * 2ページ目の手順を繰り返します

6. [構造]パネルを開き、各要素を重複クリックして、すべての構造要素がInDesignファイル内の実際の要素に対応していることを確認します。 使用されていない要素や不要な要素を削除します。 すべてのタグがセマンティックであり、要素が適切にタグ付けされていることを確認します。

   >[!NOTE]
   >
   >AEMアセットテンプレートでの問題の最も一般的な原因は、InDesignファイルの構造が不適切なことです。したがって、タグ付けと構造がクリーンで正しいことを確認してください。

## AEM Assetsでのアセットテンプレートの作成とオーサリング {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **ポート8080の開始InDesign Server** 。
2. AEM作成者インスタンスがInDesign Serverと対話操作する **（またはその逆）ように設定されていることを確認し**&#x200B;ます。

   * [IDSワーカーCloud Serviceの構成](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [クラウドプロキシCloud Serviceの設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGiの設定](http://localhost:4502/system/console/configMgr)

3. **InDesignファイルをAEM Assetsにアップロードし** 、AEMワークフローとInDesign Serverがアセットを完全に処理できるようにします。
4. **ア** セット/テンプレートの下に新しいテンプレートを作成し **** 、手順4でAEMにアップロードしたInDesignファイルを選択します。
5. **手順** 5で作成したアセットテンプレートを編集し、編集可能なフィールドを作成します。
6. 「 **完了** 」をクリックして、アセットテンプレートの最終的な高忠実度レンディションを生成します。
7. アセットテンプレートカードをクリックして開き、アセットレンディションを確認して高忠実度レンディションをダウンロードします。

## その他のリソース {#additional-resources}

InDesignテンプレートファイルとサポートする画像

InDesignテンプレートファイルとサポートする [画像のダウンロード](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC体験版のダウンロード](https://creative.adobe.com/products/download/indesign)
* [InDesign Server体験版のダウンロード](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
