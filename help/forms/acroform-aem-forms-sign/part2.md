---
title: AEM FormsとのAcroforms
seo-title: アダプティブフォームデータとAcroformの結合
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 2%

---


# Acroformからのスキーマの作成

次の手順は、前の手順で作成したAcroformからスキーマを作成することです。 このチュートリアルの一部として、スキーマを作成するサンプルアプリケーションが用意されています。 スキーマを作成するには、次の手順に従います。

1. [CRXDE Lite](http://localhost:4502/crx/de)にログイン
2. ファイル`/apps/AemFormsSamples/components/createxsd/POST.jsp`を開きます
3. `saveLocation`をハードドライブ上の適切なフォルダーに変更します。 保存先のフォルダーが既に作成されていることを確認します。
4. AEMでホストされている[Create XSD](http://localhost:4502/content/DocumentServices/CreateXsd.html)ページがブラウザーに表示されることを指定します。
5. Acroformをドラッグ&amp;ドロップします。
6. 手順3で指定したフォルダを確認します。 スキーマファイルはこの場所に保存されます。

## Acroformのアップロード

このデモがお使いのシステムで動作するには、AEM Assetsに`acroforms`という名前のフォルダを作成する必要があります。 この`acroforms`フォルダーにAcroformをアップロードします。

>[!NOTE]
>
>サンプルコードは、このフォルダー内のacroformを探します。 アダプティブフォームの送信済みデータをマージするには、Acroformが必要です。
