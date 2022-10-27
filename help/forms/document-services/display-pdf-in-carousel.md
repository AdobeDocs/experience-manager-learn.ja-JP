---
title: 複数の PDF ドキュメントの表示
description: アダプティブフォーム内の複数の PDF ドキュメントを順番に表示します。
version: 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
kt: 10292
exl-id: c1d248c3-8208-476e-b0ae-cab25575cd6a
last-substantial-update: 2021-10-12T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 6%

---

# カルーセルでの複数の PDF ドキュメントの表示

一般的な使用例としては、複数のPDFドキュメントをフォームの入力者に表示して、フォームを送信する前に確認させます。

この使用例を達成するために、 [Adobe PDF Embed API](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html).

[このサンプルのライブデモは、こちらで体験できます。](https://forms.enablementadobe.com/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)

統合を完了するために、次の手順が実行されました。

## 複数のコンポーネントドキュメントを表示するカスタムPDFを作成

PDF ドキュメントのサイクルを切り替えるカスタムコンポーネント (pdf-carousel) が作成されました。

## クライアントライブラリ

Adobe PDF Embed API を使用してPDFを表示するクライアントライブラリが作成されました。 表示するPDFは、pdf-carousel コンポーネントで指定します。

## アダプティブフォームを作成

一部のタブを使用してアダプティブフォームを作成します（このサンプルには 3 つのタブがあります）最初の 2 つのタブにアダプティブフォームのコンポーネントを追加します 3 番目のタブに pdf カルーセルコンポーネントを追加します pdf-carousel コンポーネントを設定します（下のスクリーンショットを参照）
![pdf-carousel](assets/pdf-carousel-af-component.png)

**埋め込みPDFAPI キー** - PDF の埋め込みに使用できるキーです。 このキーは localhost でのみ機能します。 次の項目を作成できます。 [自分の鍵](https://www.adobe.io/apis/documentcloud/dcsdk/pdf-embed.html) を追加し、それを他のドメインに関連付けます。

**PDF文書の指定**  — ここで、カルーセルに表示する pdf ドキュメントを指定できます。


## サーバーにサンプルをデプロイします。

ローカルサーバーでこれをテストするには、次の手順に従います。

1. [クライアントライブラリのインポート](assets/pdf-carousel-client-lib.zip) をローカルのAEMインスタンスに追加します。 [パッケージマネージャーの使用](http://localhost:4502/crx/packmgr/index.jsp)
1. [PDF カルーセルコンポーネントの読み込み](assets/pdf-carousel-component.zip) をローカルのAEMインスタンスに追加します。 [パッケージマネージャーの使用](http://localhost:4502/crx/packmgr/index.jsp)
1. [アダプティブフォームの読み込み ](assets/adaptive-form-pdf-carousel.zip) をローカルのAEMインスタンスに追加します。 [パッケージマネージャーの使用](http://localhost:4502/crx/packmgr/index.jsp)
1. [サンプル PDF を読み込んで表示します](assets/pdf-carousel-sample-documents.zip) をローカルのAEMインスタンスに追加します。 [assets ファイルのアップロードリンクの使用](http://localhost:4502/assets.html/content/dam)
1. [アダプティブフォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/wefinancecreditcard/jcr:content?wcmmode=disabled)
1. 「レビューするドキュメント」タブにタブを移動します。 カルーセルコンポーネントに 3 つのPDFドキュメントが表示されます。
