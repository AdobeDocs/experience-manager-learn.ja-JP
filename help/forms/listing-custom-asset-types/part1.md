---
title: カスタムアセットタイプの登録
description: AEM Forms Portal でのリスト用のカスタムアセットタイプの有効化
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
duration: 129
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '644'
ht-degree: 100%

---

# カスタムアセットタイプの登録 {#registering-custom-asset-types}

AEM Forms Portal でのリスト用のカスタムアセットタイプの有効化

>[!NOTE]
>
>SP1 搭載の AEM 6.3 と、対応する AEM Forms アドオンがインストールされていることを確認します。この機能は、AEM Forms 6.3 SP1 以降でのみ機能します

## 基本パスの指定 {#specify-base-path}

ベースパスは、ユーザーが検索およびリスターコンポーネントでリストする可能性があるすべてのアセットを含む、最上位のリポジトリパスです。必要に応じて、ユーザーはコンポーネント編集ダイアログからベースパス内の特定の場所を設定することもできます。これにより、ベースパス内のすべてのノードを検索するのではなく、特定の場所で検索がトリガーされます。デフォルトでは、ユーザーがこの場所内から一連の特定のパスを設定しない限り、アセットを取得するための検索パス条件としてベース パスが使用されます。パフォーマンスの高い検索を行うには、このパスに最適な値を設定することが重要です。すべての AEM Forms アセットが **_/content/dam/formsanddocuments_** にあるため、基本パスのデフォルト値は **_/content/dam/formsanddocuments_** のままです。

ベースパスの設定手順

1. crx にログインします
1. **/libs/fd/fp/extensions/querybuilder/basepath** に移動します。

1. ツールバーの「ノードのをオーバーレイ」をクリックします。
1. オーバーレイの場所が「/apps/」であることを確認します。
1. 「OK」をクリックします。
1. 「保存」をクリックする
1. **/apps/fd/fp/extensions/querybuilder/basepath** で作成された新しい構造に移動します。

1. パスプロパティの値を **「/content/dam」** に変更します。
1. 「保存」をクリックする

パスプロパティを **「/content/dam」** に指定することにより、基本的にベースパスを /content/dam に設定します。検索とリスターコンポーネントを開くと、これを確認することができます。

![basepath](assets/basepath.png)

## カスタムアセットタイプの登録 {#register-custom-asset-types}

検索とリスターコンポーネントコンポーネントに、新しいタブ（アセットリスト）が追加されました。 このタブには、標準で用意されているアセットタイプと、設定した追加のアセットタイプが一覧表示されます。 デフォルトでは、次のアセットタイプが表示されます。

1. アダプティブフォーム
1. フォームテンプレート
1. PDF forms
1. ドキュメント（静的 PDF）

**カスタムアセットタイプの登録手順**

1. **/libs/fd/fp/extensions/querybuilder/assettypes** のオーバーレイノードを作成します。

1. オーバーレイの場所を「/apps」に設定します。
1. `/apps/fd/fp/extensions/querybuilder/assettypes` で作成された新しい構造に移動します。

1. この場所の下に、登録するタイプの「nt:unstructured」ノードを作成し、そのノードに **mp4files という名前を付けます。この mp4files ノード** に以下の 2 つのプロパティを追加します。

   1. jcr:title プロパティを追加して、アセットタイプの表示名を指定します。 jcr:title の値を「Mp4 Files」に設定します。
   1. 「type」プロパティを追加し、その値を「videos」に設定します。 これが、タイプビデオのアセットを一覧表示するためにテンプレートで使用する値です。変更を保存します。

1. mp4files の下に、タイプが「nt:unstructured」のノードを作成します。 このノードに「searchcriteria」という名前を付けます。
1. 検索条件に 1 つ以上のフィルターを追加します。 例えば、ユーザーが MIME タイプが「video/mp4」である mp4Files をリストする検索フィルターが必要な場合は、ここで実行することができます。
1. ノード searchcriteria の下に、タイプ「nt:unstructured」のノードを作成します。 このノードに「filetypes」という名前を付けます。
1. 「filetypes」ノードに、次の 2 つプロパティを追加します。

   1. name: ./jcr:content/metadata/dc:format
   1. 値：video/mp4

1. これは、プロパティ dc:format が video/mp4 に等しいアセットは、アセット タイプ「Mp4 ビデオ」と見なされることを意味します。「jcr:content/metadata」ノードにリストされている任意のプロパティを、検索条件に使用することができます。

1. **作業内容を必ず保存してください**。

上記の手順を実行すると、新しいアセットタイプ（MP4 ファイル）が「検索とリスター」コンポーネントのアセットタイプドロップダウンリストに表示されるようになります（下図を参照）。

![mp4files](assets/mp4files.png)

[問題が発生してこれが機能しない場合は、下記のパッケージを読み込むことができます。](assets/assettypeskt1.zip)このパッケージには、2 つのカスタムアセットタイプが定義されています。「Mp4 Files」と「Word Documents」です。**/apps/fd/fp/extensions/querybuilder/assettypes** をご覧ください。

[customeportal パッケージをインストールします](assets/customportalpage.zip)。このパッケージには、サンプルのポータルページが含まれています。このページは、このチュートリアルの第 2 部で使用します。
