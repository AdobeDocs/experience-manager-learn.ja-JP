---
title: Adobe Analytics を使用した送信済みフォームデータフィールドに関するレポート
description: AEM Forms CS と Adobe Analytics を統合してフォームデータフィールドに関するレポートを作成する方法
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 9982e041-fff7-4be6-91c9-e322d2fd3e01
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 100%

---

# ルールの定義

タグプロパティで、2 つの新しい[ルール](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/add-data-elements-rules.html?lang=ja)（**フィールド検証エラーとフォーム送信**）を作成しました。

![adaptive-form](assets/rules.png)


## フィールド検証エラー

**フィールド検証エラー**&#x200B;ルールは、アダプティブフォームフィールドに検証エラーが発生するたびにトリガーされます。ここで扱っているフォームでは、例えば、電話番号やメールが想定される形式ではない場合、検証エラーメッセージが表示されます。

フィールド検証エラールールは、スクリーンショットに示すように、イベントを _**Adobe Experience Manager Forms - エラー**_&#x200B;にすることで設定されます。



![applicant-state-residence](assets/field_validation_error_rule.png)

「Adobe Analytics - 変数を設定」は、次のように設定します。

![アクションの設定](assets/field_validation_action_rule.png)

## フォーム送信ルール

フォーム送信ルールは、アダプティブフォームが正常に送信されるたびにトリガーされます。

フォーム送信ルールは、_**Adobe Experience Manager Forms - 送信**_&#x200B;イベントを使用して設定されます。

![form-submit-rule](assets/form-submit-rule.png)

フォーム送信ルールで、データ要素 _**ApplicatorsStateOfResidence**_ の値は prop5 にマッピングされ、データ要素 FormTitle の値は prop8 にマッピングされます。

「Adobe Analytics - 変数を設定」は、次のように設定します。
![form-submit-rule-set-variables](assets/form-submit-set-variable.png)

タグコードをテストする準備が整ったら、公開フローを使用して、[タグに加えた変更を公開します](https://experienceleague.adobe.com/docs/experience-platform/tags/publish/publishing-flow.html?lang=ja)。

## 次の手順

[ソリューションのテスト](./test.md)