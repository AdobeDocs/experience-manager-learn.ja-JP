---
title: ウェルカムキットソリューションのテスト
description: ソリューションをテストするためのソリューションアセットのデプロイ
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# サンプルアセットのデプロイとテスト

[ウェルカムキットパッケージのインストール](assets/welcomekit.zip). このパッケージには、ウェルカムキットに含めるアセット、サンプルワークフロー、電子メールテンプレート、サンプル pdf ドキュメントをリストするためのページテンプレート、カスタムコンポーネントが含まれています。
[welcome-kit バンドルのインストール](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). このバンドルには、ページを作成するコードと、Web ページに表示されるアセットを返す Java クラスが含まれています。
[サンプルのアダプティブフォームをインストールする](assets/account-openeing-form.zip)
Day CQ Mail Service を設定します。 SMTP を正しく設定する必要があるフォーム送信者に、ワークフローがウェルカムキットリンクを送信します。
必要に応じて、ワークフローの電子メールの送信コンポーネントを設定します
[フォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
詳細を入力し、1 つ以上のミューチュアルファンドを選択し、フォームを送信しますウェルカムキットへのリンクが記載された電子メールが届きます


