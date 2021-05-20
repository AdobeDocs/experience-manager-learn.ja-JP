---
title: AEM Formsを使用したAcroforms
seo-title: アダプティブフォームデータとAcroformの結合
description: 第2部は、AcroformsとAEM Formsの統合です。 Acroformからスキーマを作成します。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
source-git-commit: 451ca39511b52e90a44bba25c6739280f49a0aac
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 2%

---


# acroformからのスキーマの作成

次の手順では、前の手順で作成したAcroformからスキーマを作成します。 このチュートリアルの一部としてスキーマを作成するためのサンプルアプリケーションが用意されています。 スキーマを作成するには、次の手順に従います。

1. [CRXDE Lite](http://localhost:4502/crx/de)にログインします。
2. ファイル`/apps/AemFormsSamples/components/createxsd/POST.jsp`を開きます。
3. `saveLocation`をハードドライブ上の適切なフォルダーに変更します。 保存先のフォルダーが既に作成されていることを確認します。
4. ブラウザーで、AEMでホストされる[Create XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html)ページを参照します。
5. Acroformをドラッグ&amp;ドロップします。
6. 手順3で指定したフォルダーを確認します。 スキーマファイルはこの場所に保存されます。

## Acroformのアップロード

このデモをシステムで動作させるには、AEM Assetsに`acroforms`という名前のフォルダーを作成する必要があります。 この`acroforms`フォルダーにAcroformをアップロードします。

>[!NOTE]
>
>サンプルコードは、このフォルダー内のacroformを探します。 アダプティブフォームの送信済みデータをマージするには、acroformが必要です。
