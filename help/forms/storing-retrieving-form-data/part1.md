---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 6ae8110d4f4bc80682c35b0dab3fe7a62cad88f3
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 3%

---

# データソースの設定

AEMを使用して外部データベースとの統合を可能にする方法は多数あります。 データベース統合の最も一般的で標準的な方法の1つは、configMgrを介してApache Sling Connection Pooled DataSource設定プロパティを使用すること [です](http://localhost:4502/system/console/configMgr)。
最初の手順は、適切な [My SQLドライバーをAEMにダウンロードして展開する](https://mvnrepository.com/artifact/mysql/mysql-connector-java) ことです。
次に、Sling Connection Pooled DataSourceプロパティを設定します。 これらのプロパティは、データベースに固有です。 次のスクリーンショットは、このチュートリアルで使用する設定を示しています。 データベーススキーマは、このチュートリアルアセットの一部として提供されます。
![データソース](assets/data-source.png)

データベースにはformdataと呼ばれるテーブルが1つあり、3つの列が![data-baseの下のスクリーンショットに示されています。](assets/data-base-tables.PNG)
