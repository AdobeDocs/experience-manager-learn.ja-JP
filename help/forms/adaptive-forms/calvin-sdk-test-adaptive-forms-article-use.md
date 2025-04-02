---
title: AEM アダプティブフォームでの自動テストの使用
description: Calvin SDK を使用したアダプティブフォームの自動テスト
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
duration: 101
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '436'
ht-degree: 100%

---

# AEM アダプティブフォームでの自動テストの使用 {#using-automated-tests-with-aem-adaptive-forms}

Calvin SDK を使用したアダプティブフォームの自動テスト

Calvin SDK は、アダプティブフォームをテストするためのアダプティブフォーム開発者用のユーティリティ API です。Calvin SDK は [Hobbes.js テストフレームワーク](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)上に構築されます。Calvin SDK は、AEM Forms 6.3 以降で使用できます。

このチュートリアルでは、以下を作成します。

* テストスイート
* テストスイートには 1 つまたは複数のテストケースが含まれます
* テストケースには 1 つまたは複数のアクションが含まれます

## はじめに {#getting-started}

[パッケージマネージャーを使用したアセットのダウンロードとインストール](assets/testingadaptiveformsusingcalvinsdk1.zip)パッケージには、サンプルスクリプトといくつかのアダプティブフォームが含まれています。これらのアダプティブフォームは、AEM Forms 6.3 バージョンを使用して構築されています。AEM Forms 6.4 以降でテストする場合は、お使いのバージョンの AEM Formsに固有の新しいフォームを作成することをお勧めします。このサンプルスクリプトは、アダプティブフォームのテストに使用できる、様々な Calvin SDK の API を示しています。AEM アダプティブフォームをテストする一般的な手順は次のとおりです。

* テストする必要があるフォームに移動します
* フィールドの値を設定します
* アダプティブフォームを送信します
* エラーメッセージを確認します

パッケージ内のサンプルスクリプトは、上記のすべてのアクションを示しています。
`mortgageForm.js` のコードを見てみましょう。

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

上記のコードは、新しいテストスイートを作成します。

* この場合の TestSuite の名前は「`Mortgage Form Test`」です。
* テストスイートを含む js ファイルへの AEM の絶対パスが指定されます。
* register パラメーターを「`true`」に設定すると、テスト UI でテストスイートを使用できるようになります。

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>この機能を AEM Forms 6.4 以降でテストする場合は、新しく作成したアダプティブフォームを使用してテストしてください。パッケージに付属のアダプティブフォームを使用することはお勧めしません。

アダプティブフォームに対して実行するテストスイートにテストケースを追加できます。

* テストスイートにテストケースを追加するには、TestSuite オブジェクトの `addTestCase` メソッドを使用します。
* `addTestCase` メソッドは、TestCase オブジェクトをパラメーターとして受け取ります。
* TestCase を作成するには、`hobs.TestCase(..)` メソッドを使用します。
* メモ：最初のパラメーターは、UI に表示されるテストケースの名前です。
* テストケースを作成したら、テストケースにアクションを追加できます。
* `navigateTo` などのアクションは、テストケースに `asserts.isTrue` をアクションとして追加できます。

## 自動化されたテストの実行 {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html) テストスイートを展開して、テストを実行します。すべてが正常に実行された場合は、次の出力が表示されます。

![calvinsdk](assets/calvinimage.png)

## サンプルテストスイートを試す {#try-out-the-sample-test-suites}

サンプルパッケージには、追加のテストスイートが 3 つ含まれています。これらのテストスイートは、次に示すように、クライアントライブラリの js.txt ファイルに適切なファイルを含めることで試すことができます。

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
