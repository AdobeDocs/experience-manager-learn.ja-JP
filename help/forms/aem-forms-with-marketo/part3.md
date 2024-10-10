---
title: AEM Forms と Marketo（第 3 部）
description: AEM Forms フォームデータモデルを使用した AEM Forms と Marketo の統合に関するチュートリアル
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: 7e0d7e87d72aa1e4450649afa6a962099ceb2db4
workflow-type: ht
source-wordcount: '217'
ht-degree: 100%

---

# フォームデータモデルの作成

データソースを設定した後、次の手順では、前の手順で設定したデータソースに基づくフォームデータモデルを作成します。フォームデータモデルを作成するには、次の手順に従ってください。

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

## 次の手順

[テストのまとめ](./part4.md)
