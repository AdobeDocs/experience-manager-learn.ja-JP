---
title: AEM AssetsのInDesignファイルとアセットテンプレートについて
description: このビデオチュートリアルでは、AEM Assets のInDesignテンプレート機能で使用するアセットファイルの定義と、それに伴うすべての考慮事項について説明します。
version: 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 2%

---

# AEM AssetsのInDesignファイルとアセットテンプレートについて {#understanding-indesign-files-and-asset-templates-in-aem-assets}

このビデオチュートリアルでは、AEM Assets のInDesignテンプレート機能で使用するアセットファイルの定義と、それに伴うすべての考慮事項について説明します。

## テンプレートテンプレートファイルのInDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. をダウンロードして開きます。 [**InDesignファイルテンプレート**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **タグパネルを開き、** タグの命名規則を確認し、タグファイル内の作成可能なInDesignは既にタグ付けされていることに注意してください。 AEMでは、タグ付き要素のみが編集可能です。

   * **ウィンドウ/ユーティリティ/タグ**

3. 「ページ」で、新しいテキスト要素を追加し、「ヘッダー」というテキストを指定して、 **見出し** 段落スタイル。

   * **ウィンドウ/スタイル/段落スタイル**

   次に、 **Page2Heading.**

4. FPO ロゴ画像 ([郵便番号で指定](assets/asset-templates-tutorial-video--supporting-files.zip)) を「マスター」ページのロゴ要素に追加します。

   * **右クリック**&#x200B;を選択し、**継ぎ手/フレーム継ぎ手オプション… /コンテンツ継ぎ手/フレームを均等に埋め込み**
   [フレーム継ぎ手オプションの詳細](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)（お客様の使用例に適したもの）

5. 「ページ」のマスターテンプレートからヘッダー（ロゴと会社名）をコピーし、「ページを同じ場所に貼り付け」を使用してページをコピーします。

   * 1 ページ目で、Shift キーを押しながらmacOSをクリックするか、Shift キーを押しながら Alt キーを押しながら Windows をクリックし、マスターページから公開されたヘッダーを選択して削除します。
   * マスターページで、「 Paste in Place 」を使用してヘッダーをページ 1 にコピーします。
   * 2 ページ目の手順を繰り返します。

6. [ 構造 ] パネルを開き、各要素をダブルクリックして、すべての構造要素がInDesignファイル内の実際の要素に対応していることを確認します。 使用されていない要素や不要な要素を削除します。 すべてのタグ付けがセマンティックであり、要素が適切にタグ付けされていることを確認します。

   >[!NOTE]
   >
   >AEM Asset Templates の問題の最も一般的な原因は、InDesignファイルの構造が不適切に構築されていることです。したがって、タグ付けと構造が明確で正しいことを確認してください。

## AEM Assetsでのアセットテンプレートの作成とオーサリング {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **開始InDesign Server** （ポート 8080）。
2. 次を確認します。 **AEM オーサーインスタンスがInDesign Serverとやり取りするように設定されている**（またその逆）

   * [IDS ワーカーCloud Service設定](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [クラウドプロキシCloud Serviceの設定](http://localhost:4502/etc/cloudservices/proxy.html)
   * [AEM Externalizer OSGi の設定](http://localhost:4502/system/console/configMgr)

3. **AEM AssetsにInDesignファイルをアップロード** を使用して、AEM Workflow とInDesign Serverがアセットを完全に処理できるようにします。
4. **新しいテンプレートを作成** under **アセット/テンプレート** をクリックし、手順#4でAEMにアップロードしたInDesignファイルを選択します。
5. **アセットテンプレートの編集** 手順#5で作成し、編集可能フィールドを作成します。
6. クリック **完了** を使用して、アセットテンプレートの最終的な高品質レンディションを生成します。
7. アセットテンプレートカードをクリックして開き、アセットレンディションを確認して高品質なレンディションをダウンロードします。

## その他のリソース {#additional-resources}

InDesignテンプレートファイルとサポートする画像

ダウンロード [InDesignテンプレートファイルとサポートする画像](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [InDesignCC の体験版ダウンロード](https://creative.adobe.com/products/download/indesign)
* InDesign Serverの体験版は、 [Adobeプレリリースサイト](https://www.adobeprerelease.com/) または [CC Enterprise のお客様は、アカウント担当者に連絡して、amInDesign Server体験版ライセンスをリクエストできます](https://www.adobe.com/jp/products/indesignserver/faq.html)
