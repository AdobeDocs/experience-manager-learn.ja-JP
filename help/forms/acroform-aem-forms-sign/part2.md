---
title: AEM Formsを使用した Acroforms
seo-title: Merge Adaptive Form data with Acroform
description: 第 2 部は、Acroforms とAEM Formsの統合です。 Acroform からスキーマを作成します。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 2%

---


# acroform からスキーマを作成

次に、前の手順で作成した Acroform からスキーマを作成します。 このチュートリアルの一部としてスキーマを作成するためのサンプルアプリケーションが用意されています。 スキーマを作成するには、次の手順に従います。

1. ログイン先 [CRXDE Lite](http://localhost:4502/crx/de)
2. ファイルを開く `/apps/AemFormsSamples/components/createxsd/POST.jsp`
3. を `saveLocation` をハードドライブ上の適切なフォルダに移動します。 保存先のフォルダーが既に作成されていることを確認します。
4. ブラウザーで次の場所を指定します。 [XSD を作成](http://localhost:4502/content/DocumentServices/CreateXsd.html) AEMでホストされるページ。
5. Acroform をドラッグ&amp;ドロップします。
6. 手順 3 で指定したフォルダを確認します。 スキーマファイルはこの場所に保存されます。

## Acroform のアップロード

このデモをシステム上で動作させるには、という名前のフォルダーを作成する必要があります。 `acroforms` AEM Assets Acroform をこのファイルにアップロードします。 `acroforms` フォルダー。

>[!NOTE]
>
>サンプルコードは、このフォルダー内の acroform を探します。 アダプティブフォームの送信済みデータを結合するには、acroform が必要です。
