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
source-git-commit: 672941b4047bb0cfe8c602e3b1ab75866c10216a
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 1%

---

# ソリューションをテスト

複数のフォーム値の組み合わせを使用して、フォームをプレビューし、送信します。 Adobe Analyticsレポートでデータが表示されるまで、数分～ 30 分かかります。 prop に設定されたデータは、eVar に設定されたデータよりも早くレポートに表示されます。

## レポートスイート

Adobe Analyticsで取り込まれるフォームデータはドーナツ形式で表示されます

**州別の送信**

![applicantbystate](assets/donut.png)

フィールド検証エラー

![field-validation-error](assets/donut-field-validation.png)

## デバッグ

アダプティブフォームが、Launch 設定を含む同じ設定コンテナを使用していることを確認してください。Adobe

フォームがAdobe Analyticsにデータを送信していることを確認するには、以下の手順を実行します

* ブラウザーで開発者ツールを開きます。
* コンソールパネルで次のテキストに入力します。

```javascript
_satellite.setDebug(true)
```

コンソールウィンドウを開いたまま、フォームを操作します。 次のように表示されます。

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger の使用

を [AEP デバッガー拡張機能](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html) をブラウザーに追加して（ログインする必要があります）、より詳細なデバッグ情報を取得します。

![platform-debugger](assets/platform-debugger.png)





