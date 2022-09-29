---
title: カスタムアセットタイプの登録
seo-title: Registering Custom Asset Types
description: AEM Forms Portal でのリスト用のカスタムアセットタイプの有効化
seo-description: Enabling custom asset types for listing in AEMForms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 2%

---

# カスタムアセットタイプの登録 {#registering-custom-asset-types}

AEM Forms Portal でのリスト用のカスタムアセットタイプの有効化

>[!NOTE]
>
>AEM 6.3 と SP1、および対応するAEM Forms Add On がインストールされていることを確認します。 この機能は、AEM Forms 6.3 SP1 以降でのみ機能します

## 基本パスを指定 {#specify-base-path}

ベースパスは、ユーザーが Search &amp; Lister コンポーネントにリストしたいすべてのアセットを含む最上位のリポジトリパスです。 必要に応じて、コンポーネント編集ダイアログからベースパス内の特定の場所を設定し、ベースパス内のすべてのノードを検索するのではなく、特定の場所で検索を実行できます。 デフォルトでは、ユーザーがこの場所から特定のパスのセットを設定しない限り、ベースパスがアセットを取得するための検索パス条件として使用されます。 検索を効率的におこなうには、このパスの最適な値を持つことが重要です。 ベースパスのデフォルト値は、 **_/content/dam/formsanddocuments_** はすべてのAEM Formsアセットがに存在するので、 **_/content/dam/formsanddocuments_**

ベースパスを設定する手順

1. crx にログイン
1. に移動します。 **/libs/fd/fp/extensions/querybuilder/basepath**

1. ツールバーの「ノードをオーバーレイ」をクリックします。
1. オーバーレイの場所が「/apps/」であることを確認します。
1. 「OK」をクリックします。
1. 「保存」をクリックする
1. 次の場所で作成された新しい構造に移動します。 **/apps/fd/fp/extensions/querybuilder/basepath**

1. パスプロパティの値をに変更します。 **&quot;/content/dam&quot;**
1. 「保存」をクリックする

パスプロパティを **&quot;/content/dam&quot;** 基本的に、ベースパスを/content/dam に設定します。 これは、Search &amp; Lister コンポーネントを開くことで確認できます。

![basepath](assets/basepath.png)

## カスタムアセットタイプの登録 {#register-custom-asset-types}

Search &amp; Lister コンポーネントに新しいタブ（アセットリスト）が追加されました。 このタブには、標準で用意されているアセットタイプと、設定した追加のアセットタイプが一覧表示されます。 デフォルトでは、次のアセットタイプが表示されます

1. アダプティブフォーム
1. フォームテンプレート
1. PDF フォーム
1. ドキュメント ( 静的PDF)

**カスタムアセットタイプの登録手順**

1. 次のオーバーレイノードを作成 **/libs/fd/fp/extensions/querybuilder/assettypes**

1. オーバーレイの場所を「/apps」に設定します。
1. 次の場所で作成された新しい構造に移動します。 `/apps/fd/fp/extensions/querybuilder/assettypes`

1. この場所の下に、登録するタイプの「nt:unstructured」ノードを作成し、そのノードに名前を付けます **mp4files. この mp4files ノードに次の 2 つのプロパティを追加します。**

   1. jcr:title プロパティを追加して、アセットタイプの表示名を指定します。 jcr:title の値を「Mp4 Files」に設定します。
   1. 「type」プロパティを追加し、その値を「videos」に設定します。 これは、ビデオタイプのアセットのリストを表示するためにテンプレートで使用する値です。 変更を保存します。

1. mp4files の下に、タイプが「nt:unstructured」のノードを作成します。 このノードに「searchcriteria」という名前を付けます。
1. 検索条件に 1 つ以上のフィルターを追加します。 ユーザーが MIME タイプが「video/mp4」の mp4Files をリストする検索フィルターを使用したい場合、ここで実行できます
1. ノード searchcriteria の下に、タイプ「nt:unstructured」のノードを作成します。 このノードに「filetypes」という名前を付けます。
1. この「filetypes」ノードに次の 2 つのプロパティを追加します。

   1. name: ./jcr:content/metadata/dc:format
   1. 値：video/mp4

1. つまり、プロパティ dc:format が video/mp4 と等しいアセットは、アセットタイプ「Mp4 ビデオ」と見なされます。 「jcr:content/metadata」ノードにリストされている任意のプロパティを検索条件に使用できます

1. **作業内容を必ず保存してください**

上記の手順を実行すると、新しいアセットタイプ（Mp4 ファイル）が、以下に示すように、Search &amp; Lister コンポーネントのアセットタイプドロップダウンリストに表示され始めます

![mp4files](assets/mp4files.png)

[この機能を使用する際に問題が発生した場合は、次のパッケージをインポートできます。](assets/assettypeskt1.zip) パッケージには、2 つのカスタムアセットタイプが定義されています。 Mp4 ファイルと Worddocuments。 次をご覧ください： **/apps/fd/fp/extensions/querybuilder/assettypes**

[customeportal パッケージのインストール](assets/customportalpage.zip). このパッケージには、サンプルのポータルページが含まれています。 このページは、このチュートリアルのパート 2 で使用します
