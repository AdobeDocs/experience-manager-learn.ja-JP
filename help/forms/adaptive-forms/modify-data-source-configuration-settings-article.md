---
title: データソースの設定を変更します。
description: 「Data Source Configuration Settings」でホスト名やその他の設定を変更します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 6c63787c-e511-4764-9a03-2c85c394bcc0
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '139'
ht-degree: 0%

---

# データソース設定を変更する機能{#ability-to-modify-data-source-configuration-settings}

AEM Forms 6.4 リリースまでは、データソースを設定した後は、RESTful サービスのスキーム、ホスト、ベースパスを変更できませんでした。 これは、異なる環境に対してデータソースをテストする場合に問題が発生していました。

AEM Forms 6.5 のリリースに伴い、上記のプロパティを簡単に変更できるようになりました。 この新しい機能を使用すると、開発環境と照らし合わせてフォームデータモデルを作成でき、結果に満足したら、別の環境を指すようにプロパティを変更できます。

以下のスクリーンショットは、AEM Forms 6.4 とForms 6.5 におけるデータソース設定を示しています。

**AEM 6.4 でのデータソース設定**

![64DataSource 設定](assets/64release.gif)
**AEM 6.5 以降で編集可能なデータソース設定**
![65 データソース設定](assets/modifiabledatasource.jfif)
