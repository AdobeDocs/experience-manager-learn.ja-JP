---
title: データソースの設定を変更します。
description: 「Data Source Configuration Settings」でホスト名やその他の設定を変更します。
feature: アダプティブフォーム
version: 6.5
topic: 開発
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 2%

---


# データソース設定を変更する機能{#ability-to-modify-data-source-configuration-settings}

AEM Forms 6.4リリースまでは、データソースが設定されると、RESTfulサービスのスキーム、ホスト、ベースパスを変更できませんでした。 これは、異なる環境に対してデータソースをテストする場合に問題が発生していました。

AEM Forms 6.5のリリースで、上記のプロパティを簡単に変更できるようになりました。 この新しい機能を使用すると、開発環境に合わせてフォームデータモデルを作成でき、結果に満足したら、別の環境を指すようにプロパティを変更できます。

以下のスクリーンショットは、AEM Forms 6.4とForms 6.5におけるデータソース設定を示しています。

**AEM 6.4でのデータソースの設定**

![64DataSource Configuration ](assets/64release.gif)
**AEM 6.5以降での編集可能なデータソース設定**
![ 65DataSource Configuration](assets/modifiabledatasource.jfif)

