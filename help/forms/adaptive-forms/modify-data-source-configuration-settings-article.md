---
title: データソース設定を変更します。
description: データソース設定でホスト名やその他の設定を変更します。
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 6c63787c-e511-4764-9a03-2c85c394bcc0
last-substantial-update: 2019-06-09T00:00:00Z
duration: 31
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 100%

---

# データソース設定を変更する機能{#ability-to-modify-data-source-configuration-settings}

AEM Forms 6.4 リリースまでは、データソースをいったん設定すると、RESTful サービスのスキーム、ホスト、ベースパスを変更することができませんでした。 様々な環境に対してデータソースをテストする場合は、これが問題となりました。

AEM Forms 6.5 のリリースに伴い、上記のプロパティを簡単に変更できるようになりました。 この新しい機能を使用すると、開発環境と照らし合わせてフォームデータモデルを作成することができ、結果に満足したら、別の環境を指すようにプロパティを変更することができます。

以下のスクリーンショットは、AEM Forms 6.4 および 6.5 におけるデータソース設定を示しています。

**AEM 6.4 でのデータソース設定**

![64DataSource 設定](assets/64release.gif)
**AEM 6.5 以上で編集可能なデータソース設定**
![65DataSource 設定](assets/modifiabledatasource.jfif)
