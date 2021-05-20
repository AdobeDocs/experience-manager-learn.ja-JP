---
title: カスタムアセットタイプの登録
seo-title: カスタムアセットタイプの登録
description: AEM Forms Portalでのリスト用のカスタムアセットタイプの有効化
seo-description: AEM Forms Portalでのリスト用のカスタムアセットタイプの有効化
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 3%

---


# カスタムアセットタイプの登録{#registering-custom-asset-types}

AEM Forms Portalでのリスト用のカスタムアセットタイプの有効化

>[!NOTE]
>
>AEM 6.3(SP1)と対応するAEM Formsアドオンがインストールされていることを確認します。 この機能は、AEM Forms 6.3 SP1以降でのみ機能します

## ベースパスを指定{#specify-base-path}

ベースパスは、ユーザーがSearch &amp; Listerコンポーネントにリストする可能性のあるすべてのアセットを含む、最上位のリポジトリパスです。 必要に応じて、コンポーネント編集ダイアログからベースパス内の特定の場所を設定し、ベースパス内のすべてのノードを検索するのではなく、特定の場所で検索をトリガーすることもできます。 デフォルトでは、ベースパスがアセットを取得するための検索パス条件として使用されますが、この場所から特定のパスのセットを設定する場合を除きます。 パフォーマンスの高い検索をおこなうには、このパスの最適な値を持つことが重要です。 すべてのAEM Formsアセットは&#x200B;**_/content/dam/formsanddocuments._**&#x200B;に存在するので、ベースパスのデフォルト値は&#x200B;**_/content/dam/formsanddocuments_**&#x200B;のままです。

ベースパスの設定手順

1. crxにログインします。
1. **/libs/fd/fp/extensions/querybuilder/basepath**&#x200B;に移動します。

1. ツールバーの「ノードをオーバーレイ」をクリックします。
1. オーバーレイの場所が「/apps/」であることを確認します。
1. 「 OK 」をクリックします。
1. 「保存」をクリックします。
1. **/apps/fd/fp/extensions/querybuilder/basepath**&#x200B;に作成された新しい構造に移動します。

1. パスプロパティの値を&#x200B;**&quot;/content/dam&quot;**&#x200B;に変更します。
1. 「保存」をクリックします。

パスプロパティを&#x200B;**&quot;/content/dam&quot;**&#x200B;に指定すると、基本的に「ベースパス」を/content/damに設定します。 これは、 Search &amp; Listerコンポーネントを開くことで確認できます。

![basepath](assets/basepath.png)

## カスタムアセットタイプ{#register-custom-asset-types}の登録

Search &amp; Listerコンポーネントに新しいタブ（アセットリスト）を追加しました。 このタブには、標準で設定されているアセットタイプと、追加の設定済みアセットタイプが一覧表示されます。 デフォルトでは、次のアセットタイプが表示されます

1. アダプティブフォーム
1. フォームテンプレート
1. PDF フォーム
1. ドキュメント（スタティックPDF）

**カスタムアセットタイプの登録手順**

1. **/libs/fd/fp/extensions/querybuilder/assettypes**&#x200B;のオーバーレイノードを作成します。

1. オーバーレイの場所を「/apps」に設定します。
1. **/apps/fd/fp/extensions/querybuilder/assettypesに作成された新しい構造に移動します**

1. この場所の下に、登録するタイプの「nt:unstructured」ノードを作成し、ノードに&#x200B;**mp4filesという名前を付けます。 このmp4filesノード**&#x200B;に次の2つのプロパティを追加します。

   1. アセットタイプの表示名を指定するjcr:titleプロパティを追加します。 jcr:titleの値を「Mp4 Files」に設定します。
   1. 「type」プロパティを追加し、その値を「videos」に設定します。 これは、ビデオタイプのアセットのリストを表示するためにテンプレートで使用する値です。 変更を保存します。

1. mp4filesの下に、タイプ「nt:unstructured」のノードを作成します。 このノードに「searchcriteria」という名前を付けます。
1. 検索条件に1つ以上のフィルターを追加します。 ユーザーがMIMEタイプが「video/mp4」のmp4Filesをリストする検索フィルターを作成する場合は、ここで実行できます
1. ノードsearchcriteriaの下に、タイプ「nt:unstructured」のノードを作成します。 このノードに「filetypes」という名前を付けます。
1. この「filetypes」ノードに次の2つのプロパティを追加します。

   1. name: ./jcr:content/metadata/dc:format
   1. 値：video/mp4

1. つまり、プロパティdc:formatがvideo/mp4と等しいアセットは、アセットタイプ「Mp4ビデオ」と見なされます。 検索条件には、「jcr:content/metadata」ノードにリストされた任意のプロパティを使用できます

1. **作業内容を必ず保存してください**

上記の手順を実行すると、新しいアセットタイプ（Mp4ファイル）が、以下に示すように、Search &amp; Listerコンポーネントのアセットタイプドロップダウンリストに表示されます

![mp4files](assets/mp4files.png)

[この機能を使用する際に問題が発生した場合は、次のパッケージをインポートできます。](assets/assettypeskt1.zip) パッケージには、2つのカスタムアセットタイプが定義されています。Mp4ファイルとWorddocuments。 **/apps/fd/fp/extensions/querybuilder/assettypes**&#x200B;を参照してください。

[カスタムポータルパッケージをインストールします](assets/customportalpage.zip)。このパッケージには、サンプルのポータルページが含まれています。 このページは、このチュートリアルのパート2で使用します

