---
title: Adobe Analytics を使用した送信済みフォームデータフィールドに関するレポート
description: AEM Forms CS と Adobe Analytics を統合してフォームデータフィールドに関するレポートを作成する方法
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Integrations, Development
jira: KT-12557
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: 43665a1e-4101-4b54-a6e0-d189e825073e
duration: 38
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '203'
ht-degree: 100%

---

# ソリューションのテスト

フォーム値のいくつかの組み合わせを使用して、フォームをプレビューし送信します。Adobe Analytics レポートでデータが表示されるまで、数分～30 分かかります。prop に設定されたデータは、eVar に設定されたデータよりも早くレポートに表示されます。

## レポートスイート

Adobe Analytics でキャプチャされたフォームデータはドーナツ形式で表示されます。

**状態別の送信**

![applicantsbystate](assets/donut.png)

フィールド検証エラー

![field-validation-error](assets/donut-field-validation.png)

## デバッグ

アダプティブフォームが、Adobe Launch 設定を含んだ設定コンテナと同じものを使用していることを確認してください。

フォームが Adobe Analytics にデータを送信していることを確認するには、以下を行います。

* ブラウザーで開発者ツールを開きます。
* コンソールパネルで次のテキストを入力します。

```javascript
_satellite.setDebug(true)
```

コンソールウィンドウを開いたまま、フォームを操作します。例えば、次のように表示されます。

![console-debug](assets/debug.png)

## Adobe Experience Platform Debugger の使用

[AEP デバッガー拡張機能](https://experienceleague.adobe.com/docs/experience-platform/debugger/home.html?lang=ja)をブラウザーに追加すると（ログインする必要があります）、より詳細なデバッグ情報を取得できます。

![platform-debugger](assets/platform-debugger.png)

## これで完了です

AEM Forms as a Cloud Service と Adobe Analytics を正常に統合して、フォームデータフィールドに関するレポートを作成しました。