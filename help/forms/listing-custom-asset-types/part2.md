---
title: AEM Formsでのカスタムアセットタイプのリスト
seo-title: AEM Formsでのカスタムアセットタイプのリスト
description: Part 2 of Listing Custom Asset Types inAEM Forms
seo-description: Part 2 of Listing Custom Asset Types inAEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 0%

---


# AEM Formsでのカスタムアセットタイプのリスト {#listing-custom-asset-types-in-aem-forms}

## Creating custom template {#creating-custom-template}


この記事の目的で、カスタムアセットタイプとOOTBアセットタイプを同じページに表示するカスタムテンプレートを作成します。 カスタムテンプレートを作成するには、次の手順に従います

1. スリングの作成：/appsの下のフォルダー。 名前を「 myportalcomponent 」にします。
1. &quot;fpContentType&quot; プロパティを追加この値を「**/libs/fd/ fp/formTemplate」に設定します。**
1. 「追加title」プロパティを追加し、その値を「custom template」に設定します。 これは、Search &amp; Listerコンポーネントのドロップダウンリストに表示される名前です
1. このフォルダーの下に「template.html」を作成します。 このファイルには、様々なアセットタイプのスタイルを設定し、表示するコードが保持されます。

![appsfolder](assets/appsfolder_.png)

次のコードでは、search &amp; listerコンポーネントを使用して、様々なタイプのアセットをリストします。 アセットのタイプごとに、data-type = &quot;videos&quot;タグで示すように、個別のhtml要素を作成します。 「ビデオ」のアセットタイプの場合は、&lt;video>要素を使用してビデオをインラインで再生します。 「worddocuments」のアセットタイプでは、異なるhtmlマークアップを使用します。

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
>11行目 — 画像srcを変更して、DAMで選択した画像を指すようにしてください。
>
>このテンプレート内のアダプティブFormsをリストするには、新しいdivを作成し、そのdata-type属性を&quot;guide&quot;に設定します。 data-type=&quot;printForm&quot;のdivをコピーして貼り付け、新しくコピーしたdivのデータタイプを&quot;guide&quot;に設定できます

## Configure Search And Lister Component {#configure-search-and-lister-component}

カスタムテンプレートを定義したら、このカスタムテンプレートを「Search &amp; Lister」コンポーネントに関連付ける必要があります。 ブラウザー [にこのURLを指定し ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html)ます。

デザインモードに切り替え、許可されているコンポーネントグループにSearch &amp; Listerコンポーネントが含まれるように段落システムを設定します。 Search &amp; Listerコンポーネントは、ドキュメントサービスグループの一部です。

編集モードに切り替え、Search &amp; ListerコンポーネントをParSysに追加します。

「Search &amp; Lister」コンポーネントの設定プロパティを開きます。 「アセットフォルダ」タブが選択されていることを確認します。 Search &amp; Listerコンポーネント内のアセットのリスト元となるフォルダを選択します。 この記事の目的で、

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

「表示」タブにタブ移動します。 ここでは、Search &amp; Listerコンポーネントにアセットを表示するテンプレートを選択します。

次に示すように、ドロップダウンから「カスタムテンプレート」を選択します。

![SearchLister](assets/searchandlistercomponent.gif)

ポータルでリストするアセットのタイプを設定します。 アセットのタブの種類を「アセット一覧」に設定し、アセットの種類を設定するには この例では、次のタイプのアセットを設定します

1. MP4ファイル
1. 単語ドキュメント
1. ドキュメント（OOTBアセットタイプ）
1. Form Template(This is OOTB asset type)

次のスクリーンショットは、リスト表示用に設定されているアセットタイプを示しています

![assettypes](assets/assettypes.png)

これで、Search &amp; Listerポータルコンポーネントを設定したら、リスターの動作を確認します。 ブラウザー [にこのURLを指定し ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled)ます。 結果は次の図のようになります。

>[!NOTE]
>
>ポータルがパブリッシュサーバーでカスタムアセットタイプをリストしている場合は、ノード/apps/fd/fp/extensions/querybuilderに対して「fd-service」ユーザーに「読み取り」権限を与えていることを確認して **ください**

![assettypes](assets/assettypeslistings.png)[パッケージマネージャーを使用して、このパッケージをダウンロードしてインストールしてください。](assets/customassettypekt1.zip) これには、search &amp; listerコンポーネントを使用してリストするアセットタイプとして使用されるサンプルmp4、ワードドキュメント、xdpファイルが含まれます
