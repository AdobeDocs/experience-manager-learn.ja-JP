---
title: AEM Forms Cloud ServiceとMarketoの統合（パート 3）
description: AEM Forms フォームデータモデルを使用して AEM Forms と Marketo を統合する方法を説明します
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 77%

---

# フォームデータモデルを作成

データソースを設定した後、次の手順では、前の手順で設定したデータソースに基づいてフォームデータモデルを作成します。 フォームデータモデルを作成するには、次の手順に従ってください。

ブラウザーで[データ統合ページ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)を参照します。これには、AEM インスタンスで作成されたすべてのデータ統合が表示されます。

1. 作成 ／フォーム データモデルをクリックします。
1. FormsAndMarketo などの意味のあるタイトルを入力し、「次へ」をクリックします。
1. 前の手順で設定したデータソースを選択し、作成／編集をクリックして、フォームデータモデルを編集モードで開いて編集します。
1. 「FormsAndMarketo」ノードを展開します。 「Services」ノードを展開します。
1. 最初の「Get」操作を選択します。
1. 「選択項目を追加」をクリックします。
1. 関連モデルオブジェクトを追加ダイアログボックスで「すべて選択」をクリックし、「追加」をクリックします。
1. 「保存」ボタンをクリックしてフォームデータモデルを保存します。
1. 「サービス」タブに移動します。
1. 表示される唯一のサービスを選択し、「テストサービス」をクリックします。
1. 有効な leadId を入力し、「テスト」をクリックします。 すべてがうまくいけば、下のスクリーンショットに示すように、リードの詳細が返されます。
   ![testresults](assets/testresults.png)

このフォームデータモデルに基づいてアダプティブフォームを作成し、Marketo オブジェクトを挿入および取得できるようになりました。