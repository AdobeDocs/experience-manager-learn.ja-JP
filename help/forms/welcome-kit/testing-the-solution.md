---
title: ウェルカムキットソリューションのテスト
description: ソリューションをテストするためのソリューションアセットのデプロイ
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
duration: 29
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 100%

---

# サンプルアセットをデプロイしてテストする

[ウェルカムキットパッケージをインストール](assets/welcomekit.zip)。このパッケージには、ウェルカムキットに含めるページテンプレート、アセットを一覧表示するカスタムコンポーネント、サンプルワークフロー、メールテンプレート、サンプル PDF ドキュメントが含まれています。
[welcome-kit バンドルをインストールします](assets/welcomekit.core-1.0.0-SNAPSHOT.jar)。このバンドルには、ページを作成するコードと、web ページに表示されるアセットを返す Java クラスが含まれています。
[サンプルのアダプティブフォームをインストール](assets/account-openeing-form.zip)
Day CQ Mail Service を設定します。 SMTP を正しく設定する必要があるフォーム送信者に、ワークフローがウェルカムキットリンクを送信します。
要件に従って、ワークフローのメール送信コンポーネントを設定します。
[フォームをプレビューします。](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
詳細を入力し、1 つ以上の投資ファンドを選択し、フォームを送信します。
ウェルカムキットへのリンクが記載されたメールが届きます。
