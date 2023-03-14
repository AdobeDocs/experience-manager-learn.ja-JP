---
title: Adobe Analytics
description: AEM Forms CS とAdobe Analyticsを統合して、フォームデータフィールドに関するレポートを作成する
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 12557
source-git-commit: 439167be96959baea54f50a221c6d26f8fab78b2
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# ルールを定義

Tags プロパティで、2 つの新しい [ルール](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html)(**フィールド検証エラーとフォーム送信**) をクリックします。
![adaptive-form](assets/rules.png)


## フィールド検証エラー

この **フィールド検証エラー** ルールは、アダプティブフォームフィールドに検証エラーが発生するたびにトリガーされます。 例えば、電話番号や E メールが想定される形式ではない場合、検証エラーメッセージが表示されます。
フィールド認証エラールールは、イベントを _**Adobe Experience Manager Forms-Error**_ スクリーンショットに示すように

![出願人の住居](assets/field_validation_error_rule.png)

「 Adobe Analytics - Set Variables 」は、次のように設定します

![アクションを設定](assets/field_validation_action_rule.png)

## フォーム送信ルール

フォーム送信ルールは、アダプティブフォームが正常に送信されるたびにトリガーされます。
フォーム送信ルールは、 _**Adobe Experience Manager Forms — 送信**_ イベント

![form-submit-rule](assets/form-submit-rule.png)

フォーム送信ルールで、データ要素の値 _**ApplicatorsStateOfResidence**_ は prop5 にマッピングされ、データ要素 FormTitle の値は prop8 にマッピングされます。

Adobe Analytics - Set 変数は、次のように設定します。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

タグコードをテストする準備が整ったら、[タグに加えた変更を公開する](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html) 公開フローを使用します。
