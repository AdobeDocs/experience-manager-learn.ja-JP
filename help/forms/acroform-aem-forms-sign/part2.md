---
title: Acroforms と AEM Forms の連携
seo-title: Merge Adaptive Form data with Acroform
description: 第 2 部は、Acroforms と AEM Forms の統合です。 Acroform からスキーマを作成します。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: ht
source-wordcount: '177'
ht-degree: 100%

---


# Acroform からスキーマを作成

次に、前の手順で作成した Acroform からスキーマを作成します。 このチュートリアルの一部としてスキーマを作成するためのサンプルアプリケーションが用意されています。 スキーマを作成するには、次の手順に従います。

1. [CRXDE Lite](http://localhost:4502/crx/de) にログインします。
2. ファイル `/apps/AemFormsSamples/components/createxsd/POST.jsp` を開く
3. `saveLocation` をハードドライブ上の適切なフォルダーに変更します。 保存先のフォルダーがすでに作成されていることを確認します。
4. ブラウザーで、AEM でホストされている [XSD の作成](http://localhost:4502/content/DocumentServices/CreateXsd.html)ページを指定します。
5. Acroform をドラッグ＆ドロップします。
6. 手順 3 で指定したフォルダーを確認します。 スキーマファイルはこの場所に保存されます。

## Acroform のアップロード

このデモがシステム上で動作するには、AEM Assets で `acroforms` という名前のフォルダーを作成する必要があります。 Acroform をこの `acroforms` フォルダーにアップロードします。

>[!NOTE]
>
>サンプルコードは、このフォルダー内で Acroform を検索します。 アダプティブフォームの送信済みデータを結合するには、Acroform が必要です。
