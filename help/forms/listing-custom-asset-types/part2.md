---
title: AEM Formsでのカスタムアセットタイプのリスト
description: 第 2 部AEM Formsでのカスタムアセットタイプのリスト
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# AEM Formsでのカスタムアセットタイプのリスト {#listing-custom-asset-types-in-aem-forms}

## カスタムテンプレートの作成 {#creating-custom-template}

この記事の目的で、カスタムアセットタイプと OOTB アセットタイプを同じページに表示するカスタムテンプレートを作成します。 カスタムテンプレートを作成するには、次の手順に従ってください

1. Sling を作成します。/apps の下のフォルダー。 名前を「 myportalcomponent 」にします。
1. &quot;fpContentType&quot; プロパティを追加値を「**/libs/fd/ fp/formTemplate」と入力します。**
1. 「title」プロパティを追加し、その値を「custom template」に設定します。 これは、Search &amp; Lister コンポーネントのドロップダウンリストに表示される名前です
1. このフォルダーの下に「template.html」を作成します。 このファイルには、様々なアセットタイプのスタイルを設定および表示するコードが格納されます。

![appsfolder](assets/appsfolder_.png)

次のコードは、 search &amp; lister コンポーネントを使用する様々なタイプのアセットをリストします。 アセットのタイプごとに別々の html 要素を作成します（ data-type = &quot;videos&quot;タグで示すように）。 「ビデオ」のアセットタイプの場合、 &lt;video> 要素を使用してビデオをインラインで再生します。 「worddocuments」のアセットタイプでは、異なる HTML マークアップを使用します。

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
>11 行目 — 画像の src を、DAM で選択した画像を指すように変更してください。
>
>このテンプレート内のアダプティブFormsをリストするには、新しい div を作成し、その data-type 属性を&quot;guide&quot;に設定します。 data-type=&quot;printForm の div をコピーして貼り付け、新しくコピーした div の data-type を&quot;guide&quot;に設定できます。

## Search And Lister コンポーネントの設定 {#configure-search-and-lister-component}

カスタムテンプレートを定義したら、このカスタムテンプレートを「Search &amp; Lister」コンポーネントに関連付ける必要があります。 ブラウザーを指定 [この url に ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

デザインモードに切り替え、許可されたコンポーネントグループに Search And Lister コンポーネントを含めるように段落システムを設定します。 「Search &amp; Lister」コンポーネントは、「Document Services」グループの一部です。

編集モードに切り替え、Search &amp; Lister コンポーネントを ParSys に追加します。

「Search &amp; Lister」コンポーネントの設定プロパティを開きます。 「アセットフォルダー」タブが選択されていることを確認します。 Search &amp; Lister コンポーネント内のアセットのリストを作成するフォルダを選択します。 この記事の目的で、私は

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

「表示」タブにタブを移動します。 ここでは、Search &amp; Lister コンポーネントにアセットを表示するテンプレートを選択します。

次に示すように、ドロップダウンから「カスタムテンプレート」を選択します。

![searchandlister](assets/searchandlistercomponent.gif)

ポータルに一覧表示するアセットのタイプを設定します。 アセットのタブのタイプを「アセット一覧」に設定し、アセットのタイプを設定するには この例では、次のタイプのアセットを設定します

1. MP4 ファイル
1. Word ドキュメント
1. ドキュメント（これは OOTB アセットタイプです）
1. フォームテンプレート（これは OOTB アセットタイプ）

次のスクリーンショットは、リストに設定されているアセットタイプを示しています

![assettypes](assets/assettypes.png)

これで、Search &amp; Lister ポータルコンポーネントを設定したので、次に、操作中のリスターを表示します。 ブラウザーを指定 [この url に ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). 結果は、次の画像のようになります。

>[!NOTE]
>
>ポータルでパブリッシュサーバー上のカスタムアセットタイプがリストされている場合は、ノードに対して「fd-service」ユーザーに対する「読み取り」権限を必ず付与してください **/apps/fd/fp/extensions/querybuilder**

![assettypes](assets/assettypeslistings.png)
[パッケージマネージャーを使用して、このパッケージをダウンロードしてインストールしてください。](assets/customassettypekt1.zip) これには、Search &amp; Lister コンポーネントを使用してリスト表示するアセットタイプとして使用される、サンプル mp4 と Word ドキュメントおよび xdp ファイルが含まれます
