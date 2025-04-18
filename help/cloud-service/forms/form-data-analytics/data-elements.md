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
exl-id: b9dc505d-72c8-4b6a-974b-fc619ff7c256
duration: 42
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '138'
ht-degree: 100%

---

# データ要素の作成

タグプロパティに、2 つの新しいデータ要素（ApplientsStateOfResidence および validationError）を追加しました。

![adaptive-form](assets/data_elements.png)

## ApplicantStateOfResidence

**ApplicantStateOfResidence** データ要素は、以下のスクリーンショットに示すように、拡張子のドロップダウンで「**コア**」を選択し、データ要素タイプとして「**カスタムコード**」を選択することで設定されました。
![applicant-state-residence](assets/applicantstateofresidence.png)

次のカスタムコードは、**_ステート_**&#x200B;アダプティブフォームフィールドから値を取得するために使用されました。

```javascript
// use the GuideBridge API to access adaptive form elements
//The state field's SOM expression is used to access the state field
var ApplicantsStateOfResidence = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].state[0]").value;
_satellite.logger.log("Returning  Applicants State Of Residence is "+ApplicantsStateOfResidence);
return ApplicantsStateOfResidence;
```

## validationError

**ValidationError** データ要素は、以下のスクリーンショットに示すように、拡張子のドロップダウンで「**コア**」を選択し、データ要素タイプとして「**カスタムコード**」を選択することで設定されました。

![validation-error](assets/validation-error.png)

次のカスタムコードは、`validationError` データ要素の値を設定するために記述されました。

```javascript
var validationError = "";
// Using GuideBridge API to access adaptive forms fields using the fields SOM expression
var tel = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].telephone[0]");
var email = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].email[0]");

_satellite.logger.log("Got tel in Tags custom script "+tel.isValid)
_satellite.logger.log("Got email in Tags custom script "+email.isValid)

if (tel.isValid == false) {  
  validationError = "error: telephone number";
  _satellite.logger.log("Validation error is "+ validationError);
}

if (email.isValid == false) {  
  validationError = "error: invalid email";
  _satellite.logger.log("Validation error is "+ validationError);
}

return validationError;
```

## 次の手順

[ルールの作成](./rules.md)