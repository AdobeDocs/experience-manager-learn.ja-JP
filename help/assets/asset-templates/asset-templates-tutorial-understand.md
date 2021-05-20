---
title: 'AEM AssetsのInDesignファイルとアセットテンプレートについて '
description: このビデオチュートリアルでは、AEM AssetsのInDesignテンプレート機能で使用するアセットファイルの定義と、それに伴うすべての考慮事項について説明します。
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Intermediate
source-git-commit: b4fa992abe22e3a546d651e465d6ffc9e415aee2
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 1%

---


# AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}のInDesignファイルとアセットテンプレートについて

このビデオチュートリアルでは、AEM AssetsのInDesignテンプレート機能で使用するアセットファイルの定義と、それに伴うすべての考慮事項について説明します。

## InDesignテンプレートファイル{#constructing-the-indesign-template-file}の作成

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. [**InDesignファイルテンプレート**](assets/asset-templates-tutorial-video--supporting-files.zip)&#x200B;をダウンロードして開きます。
2. **タグパネルを開き、** タグ命名規則を確認します。タグファイル内の作成者可能な要素は既にタグ付けされていることに注意してください。AEMでは、タグ付き要素のみが編集可能です。

   * **ウィンドウ/ユーティリティ/タグ**

3. ページで、新しいテキスト要素を追加し、「Header」というテキストを指定して、**見出し**&#x200B;段落スタイルを適用します。

   * **ウィンドウ/スタイル/段落スタイル**

   次に、**Page2Heading.**&#x200B;という名前の新しいタグを作成して適用します。

4. マスターページのロゴ要素に、zip](assets/asset-templates-tutorial-video--supporting-files.zip)に指定されたFPOロゴ画像([)を追加します。

   * **右クリックし**&#x200B;て、**継ぎ手/フレーム継ぎ手オプション/コンテンツ継ぎ手/フレームを均等に埋め込みを選択します。**
   [フレームフィッティングオプションの詳細](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)、および使用事例に適したオプションについて説明します。

5. 「ページ」の「マスター」テンプレートからヘッダー（ロゴと会社名）をコピーし、「ページを同じ場所に貼り付け」を使用してページをコピーします。

   * 1ページ目で、Shift+Cmd+Click（macOSの場合）またはShift+Alt+Click（Windowsの場合）を押しながら、マスターページから公開されたヘッダーを選択して削除します。
   * マスターページから、「同じ場所に貼り付け」を使用してヘッダーをページ1にコピーします。
   * 2ページ目の手順を繰り返します。

6. [構造]パネルを開き、各要素をダブルクリックして、すべての構造要素がInDesignファイル内の実際の要素に対応していることを確認します。 使用されていない要素や不要な要素を削除します。 すべてのタグがセマンティックで、要素が正しくタグ付けされていることを確認します。

   >[!NOTE]
   >
   >AEM Asset Templatesの問題の最も一般的な原因は、InDesignファイルの構造が不完全なことです。したがって、タグ付けと構造が正しく、明確になっていることを確認してください。

## AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}でのアセットテンプレートの作成とオーサリング

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesignサ** ーバーのポート8080を起動します。
2. **AEMオーサーインスタンスがInDesign Server**&#x200B;とやり取りするように設定されている（またはその逆も同様）ことを確認します。

   * [IDSワーカーCloud Service設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [クラウドプロキシCloud Serviceの設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGiの設定](http://localhost:4502/system/console/configMgr)

3. **InDesignファイルをAEM Assetsにア** ップロードし、AEM WorkflowとInDesign Serverがアセットを完全に処理できるようにします。
4. **アセット/テンプレ** ートの下 **に新しいInDesign** を作成し、手順#4でAEMにアップロードしたテンプレートファイルを選択します。
5. **手順#5で作成し** たアセットテンプレートを編集し、編集可能フィールドを作成します。
6. 「**完了**」をクリックして、アセットテンプレートの最終的な高品質レンディションを生成します。
7. アセットテンプレートカードをクリックして開き、アセットレンディションを確認して高品質レンディションをダウンロードします。

## その他のリソース {#additional-resources}

InDesignテンプレートファイルとサポートする画像

[InDesignテンプレートファイルと、サポートする画像](assets/asset-templates-tutorial-video--supporting-files-1.zip)をダウンロードします。

* [InDesignCCの体験版ダウンロード](https://creative.adobe.com/products/download/indesign)
* [CC Enterpriseのお客様は、アカウント担当者に連絡して、InDesign Server体験版ライセンスを要求できます](https://www.adobe.com/products/indesignserver/faq.html)
