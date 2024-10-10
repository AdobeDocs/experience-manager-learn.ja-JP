---
title: AEM Forms のカスタムアセットタイプの一覧表示
description: AEM Forms のカスタムアセットタイプの一覧表示（第 2 部）
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
duration: 136
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '584'
ht-degree: 100%

---

# AEM Forms のカスタムアセットタイプの一覧表示 {#listing-custom-asset-types-in-aem-forms}

## カスタムテンプレートの作成 {#creating-custom-template}

この記事のために、カスタムアセットタイプと OOTB アセットタイプを同じページに表示するカスタムテンプレートを作成します。カスタムテンプレートを作成するには、次の手順に従ってください。

1. /apps 下に sling: フォルダーを作成します。フォルダーに「myportalcomponent」という名前を付けます。
1. 「fpContentType」プロパティを追加します。その値を「**/libs/fd/ fp/formTemplate」に設定します。**
1. 「title」プロパティを追加し、その値を「custom template」に設定します。 これは、「検索とリスター」コンポーネントのドロップダウンリストに表示される名前です。
1. このフォルダーの下に「template.html」を作成します。 このファイルには、様々なアセットタイプをスタイル設定し表示するコードが格納されます。

![appsfolder](assets/appsfolder_.png)

次のコードでは、「検索とリスター」コンポーネントを使用する様々なアセットタイプを一覧表示しています。data-type = &quot;videos&quot; タグで示されているように、アセットタイプごとに個別の HTML 要素を作成します。アセットタイプが「videos」の場合は、&lt;video> 要素を使用してビデオをインラインで再生します。 アセットタイプが「worddocuments」の場合は、別の HTML マークアップを使用します。

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>11 行目 - DAM で選択した画像を指すように画像の src を変更してください。
>
>このテンプレート内のアダプティブフォームを一覧表示するには、新しい div を作成し、その data-type 属性を「guide」に設定します。 data-type=&quot;printForm の div をコピー＆ペーストし、新しくコピーした div の data-type を「guide」に設定できます。

## 「検索とリスター」コンポーネントの設定 {#configure-search-and-lister-component}

カスタムテンプレートを定義したら、このカスタムテンプレートを「検索とリスター」コンポーネントに関連付ける必要があります。 ブラウザーで[この URL](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html) を参照します。

デザインモードに切り替え、許可されたコンポーネントグループに「検索とリスター」コンポーネントを含めるように段落システムを設定します。 Search &amp; Lister コンポーネントは、ドキュメントサービスグループの一部として含まれています。

編集モードに切り替え、Search &amp; Lister コンポーネントを ParSys に追加します。

「Search &amp; Lister」コンポーネントの設定プロパティを開きます。 「アセットフォルダー」タブが選択されていることを確認します。 Search &amp; Lister コンポーネントで一覧表示するアセットのフォルダーを選択します。この記事のために以下を選択しました。

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

「表示」タブに移動します。 ここでは、Search &amp; Lister コンポーネントでアセットを表示するテンプレートを選択します。

次に示すように、ドロップダウンから「カスタムテンプレート」を選択します。

![searchandlister](assets/searchandlistercomponent.gif)

ポータルに一覧表示するアセットのタイプを設定します。 アセットのタブのタイプを「アセット一覧表示」に設定し、アセットのタイプを設定します。この例では、次のアセットタイプを設定しました。

1. MP4 ファイル
1. Word ドキュメント
1. ドキュメント（これは OOTB アセットタイプです）
1. フォームテンプレート（これは OOTB アセットタイプです）

次のスクリーンショットは、一覧表示用に設定されているアセットタイプを示しています。

![assettypes](assets/assettypes.png)

これで Search &amp; Lister ポータルコンポーネントの設定が完了したので、次に Lister の動作を確認します。 ブラウザーで[この URL](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled) を参照します。結果は、次の画像のようになります。

>[!NOTE]
>
>パブリッシュサーバーのカスタムアセットのタイプがポータルにリストアップされている場合は、「fd-service」ユーザーに **/apps/fd/fp/extensions/querybuilder** ノードに対する「読み取り」権限を必ず付与してください。

![assettypes](assets/assettypeslistings.png)
[パッケージマネージャーを使用して、このパッケージをダウンロードしてインストールしてください。](assets/customassettypekt1.zip)これには、サンプルの mp4 および word ドキュメントと、検索およびリスター コンポーネントを使用して一覧表示するアセット タイプとして使用される xdp ファイルが含まれています。
