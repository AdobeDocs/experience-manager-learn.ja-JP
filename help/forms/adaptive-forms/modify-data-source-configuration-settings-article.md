---
title: データソースの設定を変更します。
seo-title: データソースの設定を変更します。
description: 「データソースの構成設定」で、ホスト名およびその他の設定を変更します。
seo-description: 「データソース設定」で、ホスト名およびその他の設定を変更します。
feature: Adaptive Forms
topics: form-data-model
audience: developer
doc-type: technical video
activity: setup
version: 6.5
uuid: 31e297c9-3d12-4a7a-b1ff-1e347e17b24c
discoiquuid: de227e8f-0f59-4506-828b-3b6b18b61eb1
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 1%

---


# データソース構成設定の変更{#ability-to-modify-data-source-configuration-settings}

AEM Forms6.4リリースまでは、データソースの設定後は、RESTfulサービスのスキーム、ホスト、Base Pathを変更できませんでした。 この問題は、データソースを異なる環境に対してテストする場合に発生していました。

AEM Forms6.5のリリースにより、上記の特性を簡単に変更できるようになりました。 この新しい機能を使用すると、開発環境に対してフォームデータモデルを作成でき、結果に満足したら、プロパティを変更して別の環境を指すようにできます。

以下のスクリーンショットは、AEM Forms6.4とForms6.5のデータソース設定を示しています

**AEM 6.4でのデータソースの設定**

![64DataSource ](assets/64release.gif)
**ConfigurationAEM 6.5以降での編集可能なデータソースの設定**
![65DataSourceの設定](assets/modifiabledatasource.jfif)

